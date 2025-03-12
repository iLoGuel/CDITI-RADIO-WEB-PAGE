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
  
  // Timer para simular cambios de canción (reemplazar con datos reales)
  let songUpdateTimer: ReturnType<typeof setTimeout>;
  
  function togglePlay(): void {
    if (isPlaying) {
      audio.pause();
    } else {
      isLoading = true;
      audio.play().catch((error: Error) => {
        console.error("Error al reproducir:", error);
        isLoading = false;
      });
    }
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
      });
      
      audio.addEventListener('pause', () => {
        isPlaying = false;
      });
      
      audio.addEventListener('waiting', () => {
        isLoading = true;
      });
      
      // Iniciar la simulación de información de canciones
      updateSongInfo();
    }
  });
  
  onDestroy(() => {
    // Limpiar el timer cuando el componente se destruye
    if (songUpdateTimer) clearTimeout(songUpdateTimer);
    
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
        <div class="relative flex h-3 w-3 mr-2">
          <span class="absolute inline-flex h-full w-full rounded-full bg-tertiary opacity-75 animate-ping"></span>
          <span class="relative inline-flex rounded-full h-3 w-3 bg-tertiary"></span>
        </div>
        <span class="text-sm font-semibold text-secondary">{stationName} - En Vivo</span>
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
          <h3 class="text-base font-medium text-gray-800">{currentSong}</h3>
          <p class="text-xs text-gray-500">{currentArtist}</p>
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
          <button 
            on:click={togglePlay}
            class="play-button bg-primary text-white h-12 w-12 rounded-full flex items-center justify-center shadow-md hover:bg-primary/90 transition-all hover:scale-105 active:scale-95"
            disabled={isLoading}
          >
            {#if isLoading}
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
        </div>
      </div>
    </div>
  </div>
  {:else}
  <!-- Reproductor mini (versión compacta) -->
  <div class="mini-player bg-white rounded-full shadow-md border border-gray-100 pl-3 pr-2 py-1 flex items-center">
    <div class="flex items-center mr-3">
      <div class="relative flex h-2 w-2 mr-2">
        <span class="absolute inline-flex h-full w-full rounded-full bg-tertiary opacity-75 animate-ping"></span>
        <span class="relative inline-flex rounded-full h-2 w-2 bg-tertiary"></span>
      </div>
      <span class="text-xs font-medium text-gray-700 truncate max-w-[100px]">{currentSong}</span>
    </div>
    
    <button 
      on:click={togglePlay}
      class="play-button h-8 w-8 bg-primary/10 text-primary rounded-full flex items-center justify-center hover:bg-primary/20 transition-colors"
    >
      {#if isLoading}
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