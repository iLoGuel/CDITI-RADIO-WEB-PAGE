<script>
  import logo from "$lib/images/cditi-radio-logo.webp";
  let isActive = false; // Variable para controlar el estado del menú

  function mostrarMensaje() {
    alert("¡Oops Estamos trabajando en esta función actualmente y estará disponible muy pronto. Agradecemos tu comprensión y paciencia mientras la preparamos para ti. ¡Gracias por visitarnos!");
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
    <div class={`menu ${isActive? 'active' : ''}`} on:click={toggleMenu}>
      <ul class={`transition menu__container ${isActive? 'active' : ''}`}>
        <li class="transition-transform hover:scale-105 cursor-pointer">
          <a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary rounded-xl" href="/">Inicio</a>
        </li>
        <li class="transition-transform hover:scale-105 cursor-pointer">
          <a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary rounded-xl" href="https://senarisaraldadosquebradas.blogspot.com/" target="_blank">Blog</a>
        </li>
        <li class="transition-transform hover:scale-105 cursor-pointer">
          <a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary rounded-xl" href="/" on:click={mostrarMensaje}>Podcasts</a>
        </li>
        <li class="transition-transform hover:scale-105 cursor-pointer">
          <a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary rounded-xl" href="/contacto">Contacto</a>
        </li>
      </ul>
    </div>
  </nav>
</header>

<style>
 .menu__container {
    display: flex;
    flex-direction: row;
  }

  @media (max-width: 640px) {
   .menu {
      width: 100vw;
      height: 100vh;
      position: fixed;
      background-color: transparent;
      top: 0;
      left: 0;
      z-index: -1;
    }

   .menu.active {
      background-color: rgba(0, 0, 0, 0.700);
      z-index: 0;
    }

   .menu__container {
      transform: translateX(100%);
      display: flex;
      flex-direction: column;
      padding: 1.25rem;
      gap: 1.25rem;
      background-color: white;
      margin-left: auto;
      width: 60vw;
      height: 100vh;
      border-radius: 12px 0 0 12px;
    }

   .menu__container.active {
      transform: translateX(0);
    }
  }
</style>
