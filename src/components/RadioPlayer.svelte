<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  import { browser } from '$app/environment';
  
  // URL del stream de audio (URL real de la estación)
  export let streamUrl: string = "https://stream.zeno.fm/1rmbpjhryyrtv";
  export let stationName: string = "CDITI Radio";
  export let showMiniPlayer: boolean = false; // Para controlar si se muestra la versión mini
  
  // Propiedades para el sistema de buffer y reconexión
  export let reconnectAttempts: number = 5;
  export let bufferLength: number = 3; // En segundos
  export let maxBufferLength: number = 10; // Máximo buffer en segundos (para evitar latencia)
  
  // Variables para controlar el estado del reproductor
  let isPlaying: boolean = false;
  let volume: number = 1.0;
  let isMuted: boolean = false;
  let previousVolume: number = volume;
  let dragging: boolean = false;
  
  // Estado de carga del reproductor
  let isLoading: boolean = false;
  
  // Para compatibilidad móvil
  let isMobile: boolean = false;
  
  // Sistema de reconexión y estado de conexión
  let isReconnecting: boolean = false;
  let reconnectCount: number = 0;
  let reconnectTimer: ReturnType<typeof setTimeout> | null = null;
  let wasPlayingBeforeDisconnect: boolean = false;
  let connectionStatus: 'connected' | 'disconnected' | 'reconnecting' = 'connected';
  
  // Variables para el manejo del buffer
  let bufferStatus: number = 0; // Porcentaje de buffer lleno
  let audioContext: AudioContext | null = null;
  let audioElement: HTMLAudioElement | null = null;
  let mediaSource: MediaElementAudioSourceNode | null = null;
  let analyser: AnalyserNode | null = null;
  let audioData: Uint8Array | null = null;
  let checkBufferIntervalId: ReturnType<typeof setInterval> | null = null;
  let updateVisualizationIntervalId: ReturnType<typeof setInterval> | null = null;
  
  // Intervalo para verificar la conexión del stream
  let heartbeatIntervalId: ReturnType<typeof setInterval> | null = null;
  let lastSuccessfulPlayTime: number = 0;
  let networkCheckIntervalId: ReturnType<typeof setInterval> | null = null;
  
  // Crear elemento de audio nativo
  function createAudioElement(): HTMLAudioElement {
    // Crear un nuevo elemento de audio
    const audio = document.createElement('audio');
    audio.src = streamUrl;
    audio.preload = 'auto';
    audio.crossOrigin = 'anonymous'; // Para permitir análisis de audio
    audio.volume = volume;
    
    // Agregar eventos
    audio.addEventListener('playing', handlePlaying);
    audio.addEventListener('pause', handlePause);
    audio.addEventListener('waiting', handleWaiting);
    audio.addEventListener('error', handleError);
    audio.addEventListener('canplay', handleCanPlay);
    
    // Intenta reducir la latencia en dispositivos compatibles
    if ('mozAutoplayEnabled' in audio || 'webkitAudioContext' in window) {
      try {
        // @ts-ignore - Estas propiedades no están en la definición estándar
        if (audio.mozFrameBufferLength) {
          // @ts-ignore
          audio.mozFrameBufferLength = 1024;
        }
        // @ts-ignore
        if (audio.mozSampleRate) {
          // @ts-ignore
          audio.mozSampleRate = 48000;
        }
      } catch (e) {
        console.warn('Error al configurar propiedades avanzadas de audio:', e);
      }
    }
    
    return audio;
  }
  
  // Manejadores de eventos de audio
  function handlePlaying(): void {
    isPlaying = true;
    isLoading = false;
    connectionStatus = 'connected';
    reconnectCount = 0; // Resetear contador de reconexiones
    lastSuccessfulPlayTime = Date.now(); // Registrar tiempo de reproducción exitosa
  }
  
  function handlePause(): void {
    isPlaying = false;
  }
  
  function handleWaiting(): void {
    isLoading = true;
  }
  
  function handleCanPlay(): void {
    isLoading = false;
  }
  
  function handleError(event: Event): void {
    console.error('Error en reproducción de audio:', event);
    
    // Verificar si la desconexión podría ser por problemas de red
    checkNetworkAndReconnect();
  }
  
  // Función mejorada para verificar la conexión a internet
  function checkNetworkAndReconnect(): void {
    if (browser) {
      connectionStatus = 'disconnected';
      isLoading = false;
      
      // Guardar estado de reproducción
      wasPlayingBeforeDisconnect = isPlaying;
      
      // Primero verificamos si hay conexión a internet en general
      if (!navigator.onLine) {
        console.warn('No hay conexión a internet detectada');
        // Esperamos a que se restaure la conexión
        return;
      }
      
      // Si hay conexión a internet pero falla el stream, intentamos reconectar
      console.log('Hay conexión a internet pero el stream falló, intentando reconectar...');
      if (reconnectCount < reconnectAttempts) {
        reconnectCount++;
        attemptReconnect();
      }
    }
  }
  
  // Verificar activamente la conexión al servidor de streaming
  function setupConnectionHeartbeat(): void {
    if (heartbeatIntervalId) {
      clearInterval(heartbeatIntervalId);
    }
    
    heartbeatIntervalId = setInterval(() => {
      if (!audioElement || !isPlaying) return;
      
      // Si hace más de 10 segundos que no recibimos datos y se supone que estamos reproduciendo
      const now = Date.now();
      const timeSinceLastSuccess = now - lastSuccessfulPlayTime;
      
      // Si han pasado más de 10 segundos sin reproducción exitosa
      if (isPlaying && timeSinceLastSuccess > 10000 && connectionStatus === 'connected') {
        console.warn('No se han recibido datos de audio en 10 segundos, verificando conexión...');
        connectionStatus = 'reconnecting';
        checkNetworkAndReconnect();
      }
      
      // Verificar si hay datos de audio llegando (usando el analizador)
      if (analyser && audioData && isPlaying) {
        try {
          analyser.getByteFrequencyData(audioData);
          const sum = audioData.reduce((a, b) => a + b, 0);
          
          // Si no hay datos de audio (silencio total) por un tiempo
          if (sum === 0 && isPlaying) {
            console.warn('Detectado silencio total en el stream, posible desconexión');
            
            // Verificamos si este silencio persiste por un tiempo antes de reconectar
            setTimeout(() => {
              if (isPlaying && connectionStatus === 'connected' && analyser && audioData) {
                try {
                  analyser.getByteFrequencyData(audioData);
                  const newSum = audioData.reduce((a, b) => a + b, 0);
                  
                  if (newSum === 0) {
                    console.warn('Silencio persistente, intentando reconectar...');
                    connectionStatus = 'reconnecting';
                    checkNetworkAndReconnect();
                  }
                } catch (error) {
                  console.error('Error al analizar datos de audio durante verificación:', error);
                }
              }
            }, 5000);
          } else if (sum > 0) {
            // Hay datos de audio, actualizar último tiempo de éxito
            lastSuccessfulPlayTime = now;
          }
        } catch (error) {
          console.error('Error al analizar datos de audio:', error);
        }
      }
    }, 5000);
  }
  
  // Verificar periódicamente el estado de la red
  function setupNetworkCheck(): void {
    if (networkCheckIntervalId) {
      clearInterval(networkCheckIntervalId);
    }
    
    networkCheckIntervalId = setInterval(() => {
      // Realizar una petición a un servicio conocido para verificar conectividad real
      if (browser && isPlaying) {
        fetch('https://www.google.com/favicon.ico', { 
          mode: 'no-cors',
          cache: 'no-store',
          method: 'HEAD'
        })
        .then(() => {
          // Hay conexión a internet, si estamos desconectados intentar reconectar
          if (connectionStatus === 'disconnected' && wasPlayingBeforeDisconnect) {
            console.log('Conexión a internet restaurada, intentando reconectar al stream...');
            reconnectCount = 0;
            attemptReconnect();
          }
        })
        .catch(err => {
          console.warn('Error al verificar conexión a internet:', err);
          // Si estábamos reproduciendo, marcar desconexión
          if (isPlaying) {
            connectionStatus = 'disconnected';
            wasPlayingBeforeDisconnect = true;
            stopPlayback();
          }
        });
      }
    }, 15000); // Verificar cada 15 segundos
  }
  
  // Monitorear buffer y calidad de conexión
  function setupBufferMonitoring(): void {
    if (!audioElement) return;
    
    checkBufferIntervalId = setInterval(() => {
      if (!audioElement || !isPlaying) return;
      
      try {
        // Comprobar el estado del buffer
        const buffered = audioElement.buffered;
        if (buffered.length > 0) {
          const currentTime = audioElement.currentTime;
          const bufferedEnd = buffered.end(buffered.length - 1);
          const bufferedAmount = bufferedEnd - currentTime;
          
          // Calcular el porcentaje de buffer como proporción del buffer máximo deseado
          bufferStatus = Math.min(100, (bufferedAmount / bufferLength) * 100);
          
          // Si el buffer es demasiado grande (provoca latencia), reiniciar la reproducción
          if (bufferedAmount > maxBufferLength && isPlaying) {
            const wasPlaying = isPlaying;
            audioElement.currentTime = bufferedEnd - bufferLength;
            if (wasPlaying) {
              audioElement.play().catch(err => {
                console.error('Error al ajustar buffer:', err);
              });
            }
          }
          
          // Si el buffer está vacío y debería estar reproduciendo, verificar conexión
          if (bufferedAmount < 0.5 && isPlaying && connectionStatus === 'connected') {
            console.warn('Buffer bajo, verificando conexión...');
            connectionStatus = 'reconnecting';
            isReconnecting = true;
            
            // Esperar un momento para que se llene el buffer
            setTimeout(() => {
              if (connectionStatus === 'reconnecting') {
                // Si sigue reconectando después de 5 segundos, intentar reiniciar
                attemptReconnect();
              }
            }, 5000);
          }
        }
      } catch (e) {
        console.error('Error al monitorear buffer:', e);
      }
    }, 1000);
  }
  
  // Iniciar la reproducción
  async function startPlayback(): Promise<void> {
    isLoading = true;
    
    try {
      if (!audioElement && browser) {
        // Crear elemento de audio si no existe
        audioElement = createAudioElement();
        setupAudioAnalysis();
        setupBufferMonitoring();
      }
      
      if (audioElement) {
        const playPromise = audioElement.play();
        if (playPromise !== undefined) {
          playPromise
            .then(() => {
              isPlaying = true;
              isLoading = false;
              connectionStatus = 'connected';
            })
            .catch(error => {
              console.error('Error al reproducir audio:', error);
              
              // Manejar error de reproducción automática
              if (error.name === 'NotAllowedError') {
                console.warn('Reproducción automática bloqueada por el navegador');
                isLoading = false;
              } else {
                handleError(error);
              }
            });
        }
      }
    } catch (error) {
      console.error('Error al iniciar reproducción:', error);
      isLoading = false;
      connectionStatus = 'disconnected';
    }
  }
  
  // Detener la reproducción
  function stopPlayback(): void {
    try {
      if (audioElement) {
        audioElement.pause();
        isPlaying = false;
      }
    } catch (error) {
      console.error('Error al detener reproducción:', error);
    }
  }
  
  // Alternar reproducción/pausa
  function togglePlay(): void {
    if (isPlaying) {
      stopPlayback();
    } else {
      startPlayback();
    }
  }
  
  // Intentar reconexión automática
  function attemptReconnect(): void {
    connectionStatus = 'reconnecting';
    isReconnecting = true;
    
    console.log(`Intentando reconectar (intento ${reconnectCount} de ${reconnectAttempts})...`);
    
    // Limpiar cualquier temporizador existente
    if (reconnectTimer) {
      clearTimeout(reconnectTimer);
    }
    
    // Detener y reiniciar el elemento de audio
    if (audioElement) {
      const currentVolume = audioElement.volume;
      
      try {
        audioElement.pause();
        
        // Liberar recursos y crear un nuevo elemento
        audioElement.removeEventListener('playing', handlePlaying);
        audioElement.removeEventListener('pause', handlePause);
        audioElement.removeEventListener('waiting', handleWaiting);
        audioElement.removeEventListener('error', handleError);
        audioElement.removeEventListener('canplay', handleCanPlay);
        
        // Crear nuevo elemento con URL actualizada (evita caché)
        const newUrl = streamUrl + (streamUrl.includes('?') ? '&' : '?') + 'cacheBust=' + Date.now();
        const newAudio = document.createElement('audio');
        newAudio.src = newUrl;
        newAudio.preload = 'auto';
        newAudio.crossOrigin = 'anonymous'; 
        newAudio.volume = currentVolume;
        
        newAudio.addEventListener('playing', handlePlaying);
        newAudio.addEventListener('pause', handlePause);
        newAudio.addEventListener('waiting', handleWaiting);
        newAudio.addEventListener('error', handleError);
        newAudio.addEventListener('canplay', handleCanPlay);
        
        audioElement = newAudio;
        
        // Resetear contexto de audio si es necesario
        if (audioContext && mediaSource) {
          setupAudioAnalysis();
        }
        
        if (wasPlayingBeforeDisconnect) {
          // Pequeña pausa antes de intentar reconectar para asegurar que el navegador esté listo
          setTimeout(() => {
            if (!audioElement) return;
            
            audioElement.play()
              .then(() => {
                connectionStatus = 'connected';
                isReconnecting = false;
                console.log('Reconexión exitosa');
              })
              .catch(error => {
                console.error('Error en reconexión:', error);
                
                // Programar otro intento si aún quedan
                if (reconnectCount < reconnectAttempts) {
                  reconnectTimer = setTimeout(() => {
                    attemptReconnect();
                  }, 3000);
                } else {
                  connectionStatus = 'disconnected';
                  isReconnecting = false;
                  console.error('Se agotaron los intentos de reconexión');
                }
              });
          }, 1000);
        } else {
          connectionStatus = 'connected';
          isReconnecting = false;
        }
      } catch (error) {
        console.error('Error al reiniciar audio:', error);
        
        // Programar otro intento
        if (reconnectCount < reconnectAttempts) {
          reconnectTimer = setTimeout(() => {
            attemptReconnect();
          }, 3000);
        } else {
          connectionStatus = 'disconnected';
          isReconnecting = false;
        }
      }
    } else {
      // Si no hay elemento de audio, crearlo e intentar reproducir
      audioElement = createAudioElement();
      
      if (wasPlayingBeforeDisconnect) {
        startPlayback();
      } else {
        connectionStatus = 'connected';
        isReconnecting = false;
      }
    }
  }
  
  // Forzar reconexión manual
  function forceReconnect(): void {
    wasPlayingBeforeDisconnect = isPlaying;
    reconnectCount = 0;
    attemptReconnect();
  }
  
  // Manejar cambios en la conexión de red
  function handleNetworkChange(): void {
    if (browser && navigator.onLine) {
      // Volvimos a tener conexión, intentar reconectar
      console.log('Conexión a internet restaurada (evento online)');
      if (connectionStatus === 'disconnected' && wasPlayingBeforeDisconnect) {
        console.log('Intentando reconectar al stream después de restaurar conexión...');
        reconnectCount = 0;
        attemptReconnect();
      }
    } else if (browser && !navigator.onLine) {
      // Perdimos la conexión
      console.warn('Conexión a internet perdida (evento offline)');
      connectionStatus = 'disconnected';
      wasPlayingBeforeDisconnect = isPlaying;
      stopPlayback();
    }
  }
  
  // Configurar el volumen
  function handleVolumeChange(e: Event): void {
    const target = e.target as HTMLInputElement;
    volume = parseFloat(target.value);
    
    if (audioElement) {
      audioElement.volume = volume;
    }
    
    isMuted = volume === 0;
    
    // Actualizar la variable CSS para la visualización del volumen
    if (target) {
      target.style.setProperty('--volume-percentage', `${volume * 100}%`);
    }
  }
  
  // Silenciar/reactivar el sonido
  function toggleMute(): void {
    if (isMuted) {
      volume = previousVolume;
      isMuted = false;
    } else {
      previousVolume = volume;
      volume = 0;
      isMuted = true;
    }
    
    if (audioElement) {
      audioElement.volume = volume;
    }
  }
  
  // Funciones para el control de volumen táctil
  function handleVolumeStart(): void {
    dragging = true;
  }
  
  function handleVolumeEnd(): void {
    dragging = false;
  }
  
  function handleTouchStart(): void {
    dragging = true;
  }
  
  function handleTouchEnd(): void {
    handleVolumeEnd();
  }
  
  function handleTouchMove(e: TouchEvent): void {
    if (dragging && e.touches && e.touches[0]) {
      const rangeInput = e.target as HTMLInputElement;
      if (rangeInput) {
        const rect = rangeInput.getBoundingClientRect();
        const touchX = e.touches[0].clientX - rect.left;
        const percentage = touchX / rect.width;
        volume = Math.min(Math.max(percentage, 0), 1);
        
        if (audioElement) {
          audioElement.volume = volume;
        }
        
        isMuted = volume === 0;
      }
    }
  }
  
  // Analizar datos de audio para visualización (opcional)
  function setupAudioAnalysis(): void {
    if (!browser || !window.AudioContext || !audioElement) return;
    
    try {
      // Limpiar intervalo anterior si existe
      if (updateVisualizationIntervalId) {
        clearInterval(updateVisualizationIntervalId);
      }
      
      // Crear nuevo contexto de audio si no existe
      if (!audioContext) {
        audioContext = new AudioContext();
      }
      
      // Configurar analizador
      analyser = audioContext.createAnalyser();
      analyser.fftSize = 256;
      
      // Conectar elemento de audio al analizador
      mediaSource = audioContext.createMediaElementSource(audioElement);
      mediaSource.connect(analyser);
      analyser.connect(audioContext.destination);
      
      // Crear array para datos de frecuencia
      audioData = new Uint8Array(analyser.frequencyBinCount);
      
      // Configurar intervalo para actualizar visualización
      updateVisualizationIntervalId = setInterval(() => {
        if (analyser && isPlaying && audioData) {
          analyser.getByteFrequencyData(audioData);
          // Los datos están ahora en audioData y podrían usarse para visualización
        }
      }, 100);
    } catch (error) {
      console.error('Error al configurar análisis de audio:', error);
    }
  }
  
  onMount(() => {
    // Detectar si es un dispositivo móvil
    isMobile = browser && /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
    
    // Inicializar reproductor
    if (browser) {
      audioElement = createAudioElement();
      setupBufferMonitoring();
      setupConnectionHeartbeat();  // Configurar heartbeat para verificar la conexión
      setupNetworkCheck();        // Configurar verificación periódica de red
      
      // Inicializar la variable CSS para la visualización del volumen
      setTimeout(() => {
        const volumeSlider = document.querySelector('input[type=range]') as HTMLInputElement;
        if (volumeSlider) {
          volumeSlider.style.setProperty('--volume-percentage', `${volume * 100}%`);
        }
      }, 0);
      
      // Configurar eventos de red
      window.addEventListener('online', handleNetworkChange);
      window.addEventListener('offline', handleNetworkChange);
    }
  });
  
  onDestroy(() => {
    // Limpiar eventos y recursos
    if (audioElement) {
      audioElement.pause();
      audioElement.src = '';
      audioElement.load();
      audioElement.removeEventListener('playing', handlePlaying);
      audioElement.removeEventListener('pause', handlePause);
      audioElement.removeEventListener('waiting', handleWaiting);
      audioElement.removeEventListener('error', handleError);
      audioElement.removeEventListener('canplay', handleCanPlay);
    }
    
    // Limpiar intervalos y timeouts
    if (checkBufferIntervalId) clearInterval(checkBufferIntervalId);
    if (updateVisualizationIntervalId) clearInterval(updateVisualizationIntervalId);
    if (reconnectTimer) clearTimeout(reconnectTimer);
    if (heartbeatIntervalId) clearInterval(heartbeatIntervalId);
    if (networkCheckIntervalId) clearInterval(networkCheckIntervalId);
    
    // Cerrar contexto de audio
    if (audioContext) {
      audioContext.close();
    }
    
    // Remover listeners de red
    if (browser) {
      window.removeEventListener('online', handleNetworkChange);
      window.removeEventListener('offline', handleNetworkChange);
    }
  });
</script>

<div class={`radio-player ${showMiniPlayer ? 'radio-player-mini' : 'radio-player-full'}`}>
  {#if !showMiniPlayer}
  <!-- Reproductor completo -->
  <div class="player-container bg-white rounded-xl shadow-md border border-gray-100">
    <div class="player-header bg-primary/5 px-4 py-3 flex items-center justify-between border-b border-gray-100">
      <div class="flex items-center">
        <!-- Indicador de estado -->
        <div class="relative flex h-3 w-3 mr-2">
          {#if connectionStatus === 'connected'}
            <span class="absolute inline-flex h-full w-full rounded-full bg-tertiary opacity-75 animate-ping"></span>
            <span class="relative inline-flex rounded-full h-3 w-3 bg-tertiary"></span>
          {:else if connectionStatus === 'reconnecting'}
            <span class="absolute inline-flex h-full w-full rounded-full bg-yellow-400 opacity-75 animate-ping"></span>
            <span class="relative inline-flex rounded-full h-3 w-3 bg-yellow-400"></span>
          {:else}
            <span class="absolute inline-flex h-full w-full rounded-full bg-red-500 opacity-75"></span>
            <span class="relative inline-flex rounded-full h-3 w-3 bg-red-500"></span>
          {/if}
        </div>
        <span class="text-sm font-semibold text-secondary">
          {#if connectionStatus === 'disconnected'}
            Desconectado
          {:else if connectionStatus === 'reconnecting'}
            Reconectando...
          {:else}
            Transmisión en vivo
          {/if}
        </span>
      </div>
      
      <!-- Botón de volumen y silencio -->
      <button 
        class="hover:text-primary p-1 transition-colors text-gray-500 ml-auto mr-2"
        on:click={toggleMute}
      >
        {#if isMuted || volume === 0}
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" clip-rule="evenodd" />
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2" />
          </svg>
        {:else if volume < 0.5}
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" clip-rule="evenodd" />
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072" />
          </svg>
        {:else}
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" clip-rule="evenodd" />
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.07 9.93a8 8 0 010 4.14" />
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.07 7.93a12 12 0 010 8.14" />
          </svg>
        {/if}
      </button>
    </div>
    
    <div class="player-content p-4">
      <div class="flex flex-col">
        <div class="song-info mb-3">
          <h3 class="text-base font-medium text-gray-800">
            {#if connectionStatus === 'disconnected'}
              Sin conexión
            {:else if connectionStatus === 'reconnecting'}
              Reconectando...
            {:else}
              Transmisión en vivo
            {/if}
          </h3>
          <p class="text-xs text-gray-500">
            {#if connectionStatus === 'disconnected'}
              Esperando conexión a internet
            {:else if connectionStatus === 'reconnecting'}
              Intento de reconexión en curso
            {/if}
          </p>
        </div>
        
        <!-- Indicador de buffer -->
        {#if isPlaying && connectionStatus === 'connected'}
        <div class="buffer-indicator mb-2">
          <div class="w-full h-1 bg-gray-200 rounded-full overflow-hidden">
            <div class="h-full bg-tertiary rounded-full transition-all duration-300" style="width: {bufferStatus}%"></div>
          </div>
          <div class="flex justify-between mt-1">
            <span class="text-xs text-gray-400">Buffer</span>
            <span class="text-xs text-gray-400">{Math.round(bufferStatus)}%</span>
          </div>
        </div>
        {/if}
        
        <!-- Control de volumen estático -->
        <div class="volume-control mb-4 w-full px-1">
          <div class="flex items-center justify-between text-xs text-gray-500 mb-1">
            <span class="volume-min">Volumen: Min</span>
            <span class="volume-label">{Math.round(volume * 100)}%</span>
            <span class="volume-max">Max</span>
          </div>
          <input 
            type="range" 
            min="0" 
            max="1" 
            step="0.01" 
            bind:value={volume}
            on:input={handleVolumeChange}
            on:mousedown={handleVolumeStart}
            on:mouseup={handleVolumeEnd}
            on:touchstart={handleTouchStart}
            on:touchmove={handleTouchMove}
            on:touchend={handleTouchEnd}
            class="w-full slider-progress"
            style="--volume-percentage: {volume * 100}%"
            aria-label="Control de volumen"
            title="Ajustar volumen"
          />
          <div class="text-center text-xs text-gray-400 mt-1">
            <small>Desliza para ajustar el volumen</small>
          </div>
        </div>
        
        <div class="flex justify-center">
          {#if connectionStatus === 'disconnected'}
            <button 
              on:click={forceReconnect}
              class="play-button bg-red-500 text-white h-12 w-12 rounded-full flex items-center justify-center shadow-md hover:bg-red-600 transition-all hover:scale-105 active:scale-95"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
              </svg>
            </button>
          {:else}
            <button 
              on:click={togglePlay}
              class="play-button bg-primary text-white h-12 w-12 rounded-full flex items-center justify-center shadow-md hover:bg-primary/90 transition-all hover:scale-105 active:scale-95"
              disabled={isLoading || connectionStatus === 'reconnecting'}
            >
              {#if isLoading || connectionStatus === 'reconnecting'}
                <svg class="animate-spin h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                  <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                  <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                </svg>
              {:else if isPlaying}
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 9v6m4-6v6m7-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
              {:else}
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z" />
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
              {/if}
            </button>
          {/if}
        </div>
      </div>
    </div>
  </div>
  {:else}
  <!-- Reproductor mini (versión compacta) -->
  <div class="mini-player bg-white rounded-full shadow-md border border-gray-100 pl-3 pr-2 py-1 flex items-center">
    <div class="flex items-center mr-3">
      <div class="relative flex h-2 w-2 mr-2">
        {#if connectionStatus === 'connected'}
          <span class="absolute inline-flex h-full w-full rounded-full bg-tertiary opacity-75 animate-ping"></span>
          <span class="relative inline-flex rounded-full h-2 w-2 bg-tertiary"></span>
        {:else if connectionStatus === 'reconnecting'}
          <span class="absolute inline-flex h-full w-full rounded-full bg-yellow-400 opacity-75 animate-ping"></span>
          <span class="relative inline-flex rounded-full h-2 w-2 bg-yellow-400"></span>
        {:else}
          <span class="absolute inline-flex h-full w-full rounded-full bg-red-500 opacity-75"></span>
          <span class="relative inline-flex rounded-full h-2 w-2 bg-red-500"></span>
        {/if}
      </div>
      <span class="text-xs font-medium text-gray-700 truncate max-w-[100px]">
        {#if connectionStatus === 'disconnected'}
          Sin conexión
        {:else if connectionStatus === 'reconnecting'}
          Reconectando...
        {:else}
          En vivo
        {/if}
      </span>
    </div>
    
    {#if connectionStatus === 'disconnected'}
      <button 
        on:click={forceReconnect}
        class="h-8 w-8 bg-red-100 text-red-500 rounded-full flex items-center justify-center hover:bg-red-200 transition-colors"
      >
        <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
        </svg>
      </button>
    {:else}
      <button 
        on:click={togglePlay}
        class="play-button h-8 w-8 bg-primary/10 text-primary rounded-full flex items-center justify-center hover:bg-primary/20 transition-colors"
        disabled={isLoading || connectionStatus === 'reconnecting'}
      >
        {#if isLoading || connectionStatus === 'reconnecting'}
          <svg class="animate-spin h-3 w-3" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
            <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
            <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
          </svg>
        {:else if isPlaying}
          <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 9v6m4-6v6m7-3a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
        {:else}
          <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z" />
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
        {/if}
      </button>
    {/if}
  </div>
  {/if}
</div>

<style>
  .radio-player-full {
    width: 100%;
    max-width: 400px;
    position: relative;
  }
  
  .radio-player-mini {
    width: fit-content;
    position: relative;
  }
  
  .volume-control {
    z-index: 20;
  }
  
  .volume-min, .volume-max {
    font-size: 0.7rem;
    opacity: 0.7;
  }
  
  .volume-label {
    font-weight: 500;
  }
  
  .volume-slider {
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  }
  
  input[type=range] {
    height: 6px;
    background: #e5e7eb;
    border-radius: 3px;
    -webkit-appearance: none;
    touch-action: manipulation;
  }
  
  /* Estilo para la barra de progreso del volumen */
  input[type=range].slider-progress {
    background: linear-gradient(to right, var(--color-primary, #39a900) 0%, var(--color-primary, #39a900) var(--volume-percentage), #e5e7eb var(--volume-percentage), #e5e7eb 100%);
  }
  
  input[type=range]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: var(--color-primary, #39a900);
    cursor: pointer;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  }
  
  input[type=range]::-moz-range-thumb {
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: var(--color-primary, #39a900);
    cursor: pointer;
    border: none;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  }
  
  /* Ajustes para móviles */
  @media (max-width: 768px) {
    input[type=range]::-webkit-slider-thumb {
      width: 22px;
      height: 22px;
    }
    
    input[type=range]::-moz-range-thumb {
      width: 22px;
      height: 22px;
    }
    
    input[type=range] {
      height: 8px;
    }
  }
  
  /* Estilos para el indicador de buffer */
  .buffer-indicator {
    transition: opacity 0.3s ease-in-out;
  }
</style> 