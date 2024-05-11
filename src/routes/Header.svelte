<script>
  import logo from "$lib/images/cditi-radio-logo.webp";

  // Función para mostrar el mensaje "Próximamente :D"
  function mostrarMensaje() {
    alert("La pagina está en desarrollo actualmente y estará disponible próximamente. ¡Gracias por tu paciencia! 😊");
  }
</script>

<header>
  <nav class="max-w-screen-xl m-auto w-auto py-4 px-5 flex justify-between items-center">
    <a href="/">
      <img src={logo} alt="" class="h-14 p-1 hover:p-0 transition-all hover:drop-shadow-lg">
    </a>
    <button class="sm:hidden hover:shadow p-4 font-bold hover:bg-secondary hover:text-primary transition-all rounded-xl cursor-pointer flex"><i class="fa-solid fa-bars"></i></button>
    <ul class="hidden sm:flex gap-5">
      <li><a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary transition-all rounded-xl cursor-pointer no-underline" href="/">Inicio</a></li>
      <li><a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary transition-all rounded-xl cursor-pointer no-underline" href="https://senarisaraldadosquebradas.blogspot.com/" target="_blank">Blog</a></li>
      <li><a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary transition-all rounded-xl cursor-pointer no-underline" href="/" on:click={mostrarMensaje}>Podcasts</a></li>
      <li><a class="hover:shadow py-2 px-4 font-bold hover:bg-secondary hover:text-primary transition-all rounded-xl cursor-pointer no-underline" href="/" on:click={mostrarMensaje}>Contacto</a></li>
    </ul>
  </nav>
</header>
