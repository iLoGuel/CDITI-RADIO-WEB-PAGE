<script>
  import "./styles.css";

  import logo from "$lib/images/cditi-radio-logo.webp";

  // Función para mostrar el mensaje "Próximamente :D"
  function mostrarMensaje() {
    alert(
      "La pagina está en desarrollo actualmente y estará disponible próximamente. ¡Gracias por tu paciencia! 😊"
    );
  }
</script>

<div>
  <header>
    <nav
      class="max-w-screen-xl m-auto w-auto py-4 px-5 flex justify-between items-center"
    >
      <a href="/">
        <img
          src={logo}
          alt=""
          class="h-14 hover:scale-105 transition-all hover:drop-shadow-lg"
        />
      </a>
      <button
        class="sm:hidden hover:scale-105 hover:shadow p-4 font-bold hover:bg-secondary hover:text-primary transition-all rounded-xl cursor-pointer flex"
        ><i class="fa-solid fa-bars"></i></button
      >
      <ul class="hidden sm:flex gap-5">
        <li class="hover:scale-105">
          <a
            class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary transition-all rounded-xl cursor-pointer no-underline"
            href="/">Inicio</a
          >
        </li>
        <li class="hover:scale-105">
          <a
            class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary transition-all rounded-xl cursor-pointer no-underline"
            href="https://senarisaraldadosquebradas.blogspot.com/"
            target="_blank">Blog</a
          >
        </li>
        <li class="hover:scale-105">
          <a
            class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary transition-all rounded-xl cursor-pointer no-underline"
            href="/"
            on:click={mostrarMensaje}>Podcasts</a
          >
        </li>
        <li class="hover:scale-105">
          <a
            class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary transition-all rounded-xl cursor-pointer no-underline"
            href="/"
            on:click={mostrarMensaje}>Contacto</a
          >
        </li>
      </ul>
    </nav>
  </header>

  <slot />
  
  <footer>
    <p class="text-center text-balance">
      Diseñado y desarrollado por la <span class="font-bold text-primary"
        >Fabrica de Software</span
      >
      de
      <span class="font-bold text-primary">CDITI SENA Dosquebradas</span>
    </p>
  </footer>
</div>
