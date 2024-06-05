<script>
  import logo from "$lib/images/cditi-radio-logo.webp";
  let isActive = false; // Variable para controlar el estado del menú

  function mostrarMensaje() {
    alert("¡Oops Estamos trabajando en esta función actualmente y estará disponible muy pronto. Agradecemos tu comprensión y paciencia mientras la preparamos para ti. ¡Gracias por visitarnos!");
    isActive ? toggleMenu() : '';
  }

  function toggleMenu() {
    isActive =!isActive; // Cambia el estado del menú
  }


</script>

<header>
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <nav class="max-w-screen-xl m-auto w-auto py-4 px-5 flex justify-between items-center">
    <a href="/" class="flex items-center gap-4">
      <img src={logo} alt="Logo de CDITI RADIO" class="h-14 hover:scale-105 transition-all hover:drop-shadow-lg" />
      <!-- <div class="hover:drop-shadow-lg transition-all bg-white hover:scale-105 px-3 py-2  font-bold rounded-xl animate-pulse">
        🔴En vivo
      </div> -->
    </a>
    <button class="sm:hidden hover:scale-105 hover:shadow p-4 font-bold hover:bg-secondary hover:text-primary transition-transform rounded-xl flex" on:click={toggleMenu}><i class="fa-solid fa-bars"></i></button>
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div class="{`transition-all w-screen sm:h-auto fixed top-0 left-0 bg-black opacity-50 z-40 ${isActive? ' flex h-screen':'h-auto'}`}" on:click={toggleMenu}>
    </div>
    <ul class={`transition-all fixed sm:static flex flex-col sm:flex-row bg-white rounded-l-xl z-50 sm:translate-x-0 w-80 sm:w-auto  h-screen sm:h-auto sm:bg-transparent top-0 right-0 p-4 gap-4 ${isActive? 'flex' : 'translate-x-full'}`}>
      <li class="transition-transform hover:scale-105 cursor-pointer">
        <a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary rounded-xl" href="/" on:click={isActive ? toggleMenu : ''}>Inicio</a>
      </li>
      <li class="transition-transform hover:scale-105 cursor-pointer">
        <a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary rounded-xl" href="https://senarisaraldadosquebradas.blogspot.com/" target="_blank" on:click={isActive ? toggleMenu : ''}>Blog</a>
      </li>
      <li class="transition-transform hover:scale-105 cursor-pointer">
        <a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary rounded-xl" href="/" on:click={mostrarMensaje} >Podcasts</a>
      </li>
      <li class="transition-transform hover:scale-105 cursor-pointer">
        <a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary rounded-xl" href="/contacto" on:click={isActive ? toggleMenu : ''}>Contacto</a>
      </li>
    </ul>
  </nav>
</header>

