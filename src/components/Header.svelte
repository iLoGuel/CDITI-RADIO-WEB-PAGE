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
  <nav class="max-w-screen-xl m-auto w-auto py-4 px-5 flex justify-between items-center">
    <a href="/">
      <img src={logo} alt="Logo de CDITI RADIO" class="h-14 hover:scale-105 transition-transform hover:drop-shadow-lg" />
    </a>
    <button class="sm:hidden hover:scale-105 hover:shadow p-4 font-bold hover:bg-secondary hover:text-primary transition-transform rounded-xl cursor-pointer flex" on:click={toggleMenu}><i class="fa-solid fa-bars"></i></button>
    <div class={`transition menu ${isActive? 'active' : ''}`} on:click={toggleMenu}>
      <ul class={`transition menu__container ${isActive? 'active' : ''}`}>
        <li class="hover:scale-105">
          <a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary transition-transform rounded-xl cursor-pointer no-underline" href="/">Inicio</a>
        </li>
        <li class="hover:scale-105">
          <a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary transition-transform rounded-xl cursor-pointer no-underline" href="https://senarisaraldadosquebradas.blogspot.com/" target="_blank">Blog</a>
        </li>
        <li class="hover:scale-105">
          <a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary transition-transform rounded-xl cursor-pointer no-underline" href="/" on:click={mostrarMensaje}>Podcasts</a>
        </li>
        <li class="hover:scale-105">
          <a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary transition-transform rounded-xl cursor-pointer no-underline" href="/" on:click={mostrarMensaje}>Contacto</a>
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
      backdrop-filter: none;
      z-index: -50;
      top: 0;
      left: 0;
      opacity: 0;
    }

   .menu.active {
      backdrop-filter: brightness(0.5);
      z-index: 50;
      opacity: 1;
    }

   .menu__container {
      position: relative;
      transform: translateX(100%);
      display: flex;
      flex-direction: column;
      padding: 1.25rem;
      gap: 1.25rem;
      background-color: white;
      margin-left: auto;
      width: 60vw;
      height: 100vh;
      z-index: 100;
    }

   .menu__container.active {
      transform: translateX(0);
    }
  }
</style>
