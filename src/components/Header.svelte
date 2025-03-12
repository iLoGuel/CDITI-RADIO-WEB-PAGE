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

<header class={`sticky top-0 z-50 w-full transition-all duration-300 ${scrolled ? 'bg-white/95 shadow-sm backdrop-blur-sm border-b border-gray-100' : 'bg-white'}`}>
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
        <div class="flex items-center ml-3 bg-primary/10 px-2.5 py-1 rounded-full">
          <div class="relative flex h-2 w-2 mr-2">
            <span class="absolute inline-flex h-full w-full rounded-full bg-tertiary opacity-75 animate-ping"></span>
            <span class="relative inline-flex rounded-full h-2 w-2 bg-tertiary"></span>
          </div>
          <span class="text-xs font-medium text-primary">En vivo</span>
        </div>
      </div>
      
      <!-- Menú de navegación para pantallas medianas y grandes -->
      <ul class="hidden md:flex items-center space-x-4">
        <li>
          <a 
            href="/" 
            class="flex items-center px-4 py-2 text-gray-700 font-medium rounded-xl transition-all hover:text-primary hover:bg-gray-50 hover:scale-105 focus-visible:outline-primary"
            aria-current="page"
          >
            Inicio
          </a>
        </li>
        <li>
          <a 
            href="/radio" 
            class="flex items-center px-4 py-2 text-gray-700 font-medium rounded-xl transition-all hover:text-primary hover:bg-gray-50 hover:scale-105 focus-visible:outline-primary"
          >
            Radio
          </a>
        </li>
        <li>
          <a 
            href="https://senarisaraldadosquebradas.blogspot.com/"
            target="_blank"
            rel="noopener noreferrer" 
            class="flex items-center px-4 py-2 text-gray-700 font-medium rounded-xl transition-all hover:text-primary hover:bg-gray-50 hover:scale-105 focus-visible:outline-primary"
          >
            Blog
            <svg class="w-3.5 h-3.5 ms-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 18 18">
              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11v4.833A1.166 1.166 0 0 1 13.833 17H2.167A1.167 1.167 0 0 1 1 15.833V4.167A1.166 1.166 0 0 1 2.167 3h4.618m4.447-2H17v5.768M9.111 8.889l7.778-7.778"/>
            </svg>
          </a>
        </li>
        <li>
          <button 
            on:click={mostrarMensaje}
            class="flex items-center px-4 py-2 text-gray-700 font-medium rounded-xl transition-all hover:text-primary hover:bg-gray-50 hover:scale-105 focus-visible:outline-primary"
          >
            Podcasts
          </button>
        </li>
        <li>
          <a 
            href="/contacto" 
            class="flex items-center px-4 py-2 text-secondary bg-white border border-primary/30 font-medium rounded-xl transition-all hover:bg-primary/10 hover:border-primary hover:text-primary hover:scale-105 focus-visible:outline-primary"
          >
            Contacto
          </a>
        </li>
      </ul>
      
      <!-- Botón de menú móvil -->
      <button 
        type="button" 
        class="md:hidden inline-flex items-center justify-center p-2 rounded-xl text-gray-700 hover:bg-gray-100 hover:text-primary transition-all focus-visible:outline-primary"
        aria-controls="mobile-menu" 
        aria-expanded={isMenuOpen}
        on:click={toggleMenu}
      >
        <span class="sr-only">Abrir menú principal</span>
        {#if isMenuOpen}
          <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" aria-hidden="true">
            <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12"></path>
          </svg>
        {:else}
          <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" aria-hidden="true">
            <path stroke-linecap="round" stroke-linejoin="round" d="M4 6h16M4 12h16M4 18h16"></path>
          </svg>
        {/if}
      </button>
    </nav>
  </div>
  
  <!-- Menú móvil -->
  <div 
    class="fixed inset-0 bg-black/30 md:hidden transition-opacity duration-300 z-40 {isMenuOpen ? 'opacity-100' : 'opacity-0 pointer-events-none'}"
    on:click={toggleMenu}
    aria-hidden="true"
  ></div>
  
  <div 
    id="mobile-menu" 
    class="fixed top-0 right-0 h-full w-4/5 max-w-xs bg-white shadow-xl z-50 transform transition-transform duration-300 ease-in-out {isMenuOpen ? 'translate-x-0' : 'translate-x-full'} md:hidden"
  >
    <div class="p-5 border-b border-gray-100">
      <div class="flex items-center justify-between">
        <div class="flex items-center">
          <img src={logo} alt="Logo de CDITI RADIO" class="h-10 w-auto" />
        </div>
        <button 
          type="button" 
          class="inline-flex items-center justify-center p-2 rounded-xl text-gray-500 hover:bg-gray-100 hover:text-primary focus-visible:outline-primary"
          on:click={toggleMenu}
        >
          <span class="sr-only">Cerrar menú</span>
          <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" aria-hidden="true">
            <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12"></path>
          </svg>
        </button>
      </div>
    </div>
    <nav class="p-5 space-y-3">
      <a 
        href="/" 
        class="block px-4 py-3 text-base font-medium text-gray-700 rounded-xl hover:bg-gray-50 hover:text-primary transition-colors"
        on:click={toggleMenu}
      >
        Inicio
      </a>
      <a 
        href="/radio" 
        class="block px-4 py-3 text-base font-medium text-gray-700 rounded-xl hover:bg-gray-50 hover:text-primary transition-colors"
        on:click={toggleMenu}
      >
        Radio
      </a>
      <a 
        href="https://senarisaraldadosquebradas.blogspot.com/" 
        target="_blank"
        rel="noopener noreferrer"
        class="flex items-center justify-between px-4 py-3 text-base font-medium text-gray-700 rounded-xl hover:bg-gray-50 hover:text-primary transition-colors"
        on:click={toggleMenu}
      >
        <span>Blog</span>
        <svg class="w-3.5 h-3.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 18 18">
          <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11v4.833A1.166 1.166 0 0 1 13.833 17H2.167A1.167 1.167 0 0 1 1 15.833V4.167A1.166 1.166 0 0 1 2.167 3h4.618m4.447-2H17v5.768M9.111 8.889l7.778-7.778"/>
        </svg>
      </a>
      <button 
        on:click={mostrarMensaje}
        class="w-full text-left px-4 py-3 text-base font-medium text-gray-700 rounded-xl hover:bg-gray-50 hover:text-primary transition-colors"
      >
        Podcasts
      </button>
      <a 
        href="/contacto" 
        class="block px-4 py-3 text-base font-medium text-secondary border border-primary/20 rounded-xl hover:border-primary/50 hover:bg-primary/5 hover:text-primary transition-colors"
        on:click={toggleMenu}
      >
        Contacto
      </a>
    </nav>
  </div>
</header>

