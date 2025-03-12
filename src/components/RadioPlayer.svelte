<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  
  // URL del stream de audio (reemplazar con la URL real de tu estación)
  export let streamUrl: string = "https://streaming.radio.co/example-station";
  export let stationName: string = "CDITI Radio";
  export let showMiniPlayer: boolean = false; // Para controlar si se muestra la versión mini
  
  let audio: HTMLAudioElement;
  let isPlaying: boolean = false;
  let volume: number = 1.0; // Cambiado a 1.0 (100%)
  let isMuted: boolean = false;
  let previousVolume: number = volume;
  let dragging: boolean = false;
  let showVolumeSlider: boolean = false;
  
  // Estado de carga del reproductor
  let isLoading: boolean = false;
  
  // Información de la canción (puede venir de una API)
  let currentSong: string = "Cargando información...";
  let currentArtist: string = "";
  
  // Para compatibilidad móvil
  let isMobile: boolean = false;
  
  // Sistema de reconexión
  let isReconnecting: boolean = false;
  let reconnectAttempts: number = 0;
  let maxReconnectAttempts: number = 5;
  let reconnectInterval: number = 5000; // 5 segundos entre intentos
  let reconnectTimer: ReturnType<typeof setTimeout>;
  let wasPlayingBeforeDisconnect: boolean = false;
  let connectionStatus: 'connected' | 'disconnected' | 'reconnecting' = 'connected';
  
  // Timer para simular cambios de canción (reemplazar con datos reales)
  let songUpdateTimer: ReturnType<typeof setTimeout>;
  
  function togglePlay(): void {
    if (isPlaying) {
      audio.pause();
    } else {
      playAudio();
    }
  }
  
  function playAudio(): void {
    isLoading = true;
    audio.play().catch((error: Error) => {
      console.error("Error al reproducir:", error);
      isLoading = false;
      
      // Si hay un error de red, intentar reconectar
      if (navigator.onLine === false || error.name === 'NotSupportedError' || error.message.includes('network')) {
        startReconnection();
      }
    });
  }
  
  function toggleMute(): void {
    if (isMuted) {
      volume = previousVolume;
      isMuted = false;
    } else {
      previousVolume = volume;
      volume = 0;
      isMuted = true;
    }
    audio.volume = volume;
  }
  
  function handleVolumeChange(e: Event): void {
    const target = e.target as HTMLInputElement;
    volume = parseFloat(target.value);
    audio.volume = volume;
    isMuted = volume === 0;
    
    // Actualizar la variable CSS para la visualización del volumen
    if (target) {
      target.style.setProperty('--volume-percentage', `${volume * 100}%`);
    }
  }
  
  function handleVolumeStart(): void {
    dragging = true;
  }
  
  function handleVolumeEnd(): void {
    dragging = false;
  }
  
  // Funciones para soporte táctil
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
        audio.volume = volume;
        isMuted = volume === 0;
      }
    }
  }
  
  // Sistema de reconexión automática
  function startReconnection(): void {
    if (isReconnecting) return;
    
    wasPlayingBeforeDisconnect = isPlaying;
    connectionStatus = 'reconnecting';
    isReconnecting = true;
    reconnectAttempts = 0;
    
    attemptReconnect();
  }
  
  function attemptReconnect(): void {
    if (reconnectAttempts >= maxReconnectAttempts) {
      isReconnecting = false;
      connectionStatus = 'disconnected';
      console.log("Número máximo de intentos de reconexión alcanzado");
      return;
    }
    
    reconnectAttempts++;
    console.log(`Intento de reconexión ${reconnectAttempts}/${maxReconnectAttempts}`);
    
    // Reiniciar el audio
    if (audio) {
      // Parar el audio actual
      audio.pause();
      
      // Recargar el stream
      audio.load();
      
      // Si estaba reproduciendo antes, intentar reproducir de nuevo
      if (wasPlayingBeforeDisconnect) {
        audio.play().then(() => {
          isReconnecting = false;
          connectionStatus = 'connected';
          console.log("Reconexión exitosa");
        }).catch(error => {
          console.error("Error al reconectar:", error);
          
          // Programar otro intento
          reconnectTimer = setTimeout(() => {
            attemptReconnect();
          }, reconnectInterval);
        });
      } else {
        isReconnecting = false;
      }
    }
  }
  
  function handleNetworkChange(): void {
    if (navigator.onLine) {
      // Volvimos a tener conexión, intentar reconectar
      if (connectionStatus === 'disconnected') {
        startReconnection();
      }
    } else {
      // Perdimos la conexión
      connectionStatus = 'disconnected';
      if (isPlaying) {
        wasPlayingBeforeDisconnect = true;
      }
    }
  }
  
  // Función para obtener información de la canción actual (simulada)
  function updateSongInfo(): void {
    // Aquí podrías hacer una llamada API para obtener la información real
    // Para este ejemplo, usaremos datos estáticos alternados
    
    const songs = [
      { title: "Entrevista Especial", artist: "Programa Matutino" },
      { title: "Últimas Noticias", artist: "Noticiero CDITI" },
      { title: "Temas de Tecnología", artist: "TechTalk CDITI" },
      { title: "Lo Mejor del SENA", artist: "Radio CDITI" }
    ];
    
    const randomSong = songs[Math.floor(Math.random() * songs.length)];
    currentSong = randomSong.title;
    currentArtist = randomSong.artist;
    
    // Actualizar cada 30 segundos (simulando cambios de programa)
    songUpdateTimer = setTimeout(updateSongInfo, 30000);
  }
  
  onMount(() => {
    // Detectar si es un dispositivo móvil
    isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
    
    if (audio) {
      audio.volume = volume;
      
      // Inicializar la variable CSS para la visualización del volumen
      setTimeout(() => {
        const volumeSlider = document.querySelector('input[type=range]') as HTMLInputElement;
        if (volumeSlider) {
          volumeSlider.style.setProperty('--volume-percentage', `${volume * 100}%`);
        }
      }, 0);
      
      // Manejadores de eventos para el elemento audio
      audio.addEventListener('playing', () => {
        isPlaying = true;
        isLoading = false;
        connectionStatus = 'connected';
      });
      
      audio.addEventListener('pause', () => {
        isPlaying = false;
      });
      
      audio.addEventListener('waiting', () => {
        isLoading = true;
      });
      
      audio.addEventListener('error', (e) => {
        console.error("Error de audio:", e);
        if (navigator.onLine !== false) { // Solo si no sabemos ya que estamos desconectados
          startReconnection();
        }
      });
      
      audio.addEventListener('stalled', () => {
        console.log("Audio estancado, intentando reconectar");
        startReconnection();
      });
      
      // Eventos de red
      window.addEventListener('online', handleNetworkChange);
      window.addEventListener('offline', handleNetworkChange);
      
      // Iniciar la simulación de información de canciones
      updateSongInfo();
    }
  });
  
  onDestroy(() => {
    // Limpiar el timer cuando el componente se destruye
    if (songUpdateTimer) clearTimeout(songUpdateTimer);
    if (reconnectTimer) clearTimeout(reconnectTimer);
    
    // Remover listeners de red
    window.removeEventListener('online', handleNetworkChange);
    window.removeEventListener('offline', handleNetworkChange);
    
    // Detener audio y limpiar eventos
    if (audio) {
      audio.pause();
      audio.src = '';
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
          {stationName} - 
          {#if connectionStatus === 'connected'}
            En Vivo
          {:else if connectionStatus === 'reconnecting'}
            Reconectando...
          {:else}
            Desconectado
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
              {currentSong}
            {/if}
          </h3>
          <p class="text-xs text-gray-500">
            {#if connectionStatus === 'disconnected'}
              Esperando conexión a internet
            {:else if connectionStatus === 'reconnecting'}
              Intento {reconnectAttempts}/{maxReconnectAttempts}
            {:else}
              {currentArtist}
            {/if}
          </p>
        </div>
        
        <!-- Control de volumen estático -->
        <div class="volume-control mb-4 w-full px-1">
          <div class="flex items-center justify-between text-xs text-gray-500 mb-1">
            <span class="volume-min">Min</span>
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
          />
        </div>
        
        <div class="flex justify-center">
          {#if connectionStatus === 'disconnected'}
            <button 
              on:click={startReconnection}
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
          {currentSong}
        {/if}
      </span>
    </div>
    
    {#if connectionStatus === 'disconnected'}
      <button 
        on:click={startReconnection}
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
  
  <!-- Elemento de audio oculto -->
  <audio 
    bind:this={audio}
    src={streamUrl}
    preload="none"
  ></audio>
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
  
  /* Estilo para la barra de progreso */
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
</style> 