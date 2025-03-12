<script>
  import logo from "$lib/images/cditi-radio-logo.webp";
  import { onMount } from 'svelte';
  
  let isMenuOpen = false;
  let scrolled = false;
  let innerWidth = 0;
  
  // Función para controlar el menú móvil
  function toggleMenu() {
    isMenuOpen = !isMenuOpen;
  }
  
  // Función para mostrar mensaje cuando una función no está disponible
  function mostrarMensaje() {
    alert("¡Oops! Estamos trabajando en esta función actualmente y estará disponible muy pronto. Agradecemos tu comprensión y paciencia mientras la preparamos para ti. ¡Gracias por visitarnos!");
    if (isMenuOpen) toggleMenu();
  }
  
  // Cerrar menú automáticamente si la pantalla se agranda
  $: if (innerWidth >= 768 && isMenuOpen) {
    isMenuOpen = false;
  }
  
  // Detectar scroll para cambiar la apariencia del navbar
  onMount(() => {
    const handleScroll = () => {
      scrolled = window.scrollY > 20;
    };
    
    window.addEventListener('scroll', handleScroll);
    
    return () => {
      window.removeEventListener('scroll', handleScroll);
    };
  });
</script>

<svelte:window bind:innerWidth />

<header class={`sticky top-0 z-50 w-full transition-all duration-300 ${scrolled ? 'bg-white/95 shadow-sm backdrop-blur-sm border-b border-tertiary/20' : 'bg-white'}`}>
  <!-- Franja superior decorativa con gradiente de colores de la paleta -->
  <div class="h-1 w-full bg-gradient-to-r from-primary via-tertiary to-alternative"></div>
  
  <div class="max-w-screen-xl mx-auto px-4 sm:px-6 lg:px-8">
    <nav class="flex justify-between items-center h-16 md:h-20">
      <!-- Logo y En Vivo -->
      <div class="flex items-center">
        <a href="/" class="flex items-center gap-3 focus-visible:outline-primary" aria-label="CDITI RADIO - Página principal">
          <img 
            src={logo} 
            alt="Logo de CDITI RADIO" 
            class="h-12 md:h-14 transition-transform duration-300 hover:scale-105"
          />
        </a>
        
        <a 
          href="/radio" 
          class="hidden md:flex items-center ml-6 px-3 py-1.5 text-xs font-medium text-white rounded-full bg-primary hover:bg-primary/90 transition-colors"
        >
          <span class="mr-1.5 relative flex h-2 w-2">
            <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-white/70 opacity-75"></span>
            <span class="relative inline-flex rounded-full h-2 w-2 bg-white"></span>
          </span>
          En Vivo
        </a>
      </div>
      
      <!-- Navegación Desktop -->
      <div class="hidden md:flex items-center gap-1">
        <a 
          href="/" 
          class="relative px-3 py-2 text-secondary hover:text-primary transition-colors after:absolute after:left-0 after:bottom-0 after:h-0.5 after:w-0 hover:after:w-full after:bg-primary after:transition-all"
        >
          Inicio
        </a>
        <a 
          href="/radio" 
          class="relative px-3 py-2 text-secondary hover:text-primary transition-colors after:absolute after:left-0 after:bottom-0 after:h-0.5 after:w-0 hover:after:w-full after:bg-primary after:transition-all"
        >
          Radio
        </a>
        <a 
          href="/contacto" 
          class="relative px-3 py-2 text-secondary hover:text-primary transition-colors after:absolute after:left-0 after:bottom-0 after:h-0.5 after:w-0 hover:after:w-full after:bg-primary after:transition-all"
        >
          Contacto
        </a>
        <a 
          href="#" 
          class="ml-2 px-4 py-2 text-white bg-secondary hover:bg-secondary/90 rounded-md transition-colors"
          on:click|preventDefault={mostrarMensaje}
        >
          Programas
        </a>
      </div>
      
      <!-- Botón de menú móvil -->
      <button 
        type="button" 
        class="md:hidden inline-flex items-center justify-center p-2 rounded-md text-secondary hover:text-primary hover:bg-tertiary/10 transition-colors"
        aria-expanded={isMenuOpen} 
        aria-label="Toggle menu"
        on:click={toggleMenu}
      >
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
          {#if isMenuOpen}
            <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
          {:else}
            <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5" />
          {/if}
        </svg>
      </button>
    </nav>
  </div>
  
  <!-- Menú móvil -->
  {#if isMenuOpen}
    <div class="md:hidden bg-white border-t border-tertiary/10 shadow-lg">
      <div class="px-4 pt-2 pb-3 space-y-1">
        <a 
          href="/" 
          class="block px-3 py-2 rounded-md text-base font-medium text-secondary hover:bg-primary/5 hover:text-primary transition-colors"
          on:click={toggleMenu}
        >
          Inicio
        </a>
        <a 
          href="/radio" 
          class="block px-3 py-2 rounded-md text-base font-medium text-secondary hover:bg-primary/5 hover:text-primary transition-colors"
          on:click={toggleMenu}
        >
          Radio
        </a>
        <a 
          href="/contacto" 
          class="block px-3 py-2 rounded-md text-base font-medium text-secondary hover:bg-primary/5 hover:text-primary transition-colors"
          on:click={toggleMenu}
        >
          Contacto
        </a>
        <a 
          href="#" 
          class="flex items-center px-3 py-2 rounded-md text-base font-medium text-secondary hover:bg-primary/5 hover:text-primary transition-colors"
          on:click|preventDefault={mostrarMensaje}
        >
          Programas
          <span class="ml-2 px-1.5 py-0.5 text-xs font-semibold text-white bg-alternative rounded">Pronto</span>
        </a>
      </div>
      
      <!-- En Vivo (móvil) -->
      <div class="px-4 py-3 border-t border-tertiary/20">
        <a 
          href="/radio" 
          class="flex items-center justify-center px-4 py-2 rounded-md text-sm font-medium text-white bg-primary hover:bg-primary/90 transition-colors"
          on:click={toggleMenu}
        >
          <span class="mr-1.5 relative flex h-2 w-2">
            <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-white/70 opacity-75"></span>
            <span class="relative inline-flex rounded-full h-2 w-2 bg-white"></span>
          </span>
          Escuchar en Vivo
        </a>
      </div>
    </div>
  {/if}
</header>

