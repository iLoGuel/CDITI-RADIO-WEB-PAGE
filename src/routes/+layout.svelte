<script>
  import "./styles.css";

  import logo from "$lib/images/cditi-radio-logo.webp";
  import logoSennova from "$lib/images/sennova-logo.png";
  import logoSena from "$lib/images/sena-logo.svg";

  // Función para mostrar el mensaje "Próximamente :D"
  function mostrarMensaje() {
    alert(
      "¡Oops! Estamos trabajando en esta función actualmente y estará disponible muy pronto. Agradecemos tu comprensión y paciencia mientras la preparamos para ti. ¡Gracias por visitarnos!"
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
          alt="Logo de CDITI RADIO"
          class="h-14 hover:scale-105 transition-all hover:drop-shadow-lg"
        />
      </a>
      <button
        class="sm:hidden hover:scale-105 hover:shadow p-4 font-bold hover:bg-secondary hover:text-primary transition-all rounded-xl cursor-pointer flex"
        on:click={mostrarMensaje}><i class="fa-solid fa-bars"></i></button
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
  <footer class="bg-primary py-10 text-white">
    <div class="max-w-7xl px-5 mx-auto flex gap-5 justify-center lg:justify-between items-center flex-wrap">
      <p class="text-sm">
        Servicio Nacional de Aprendizaje SENA-CENTRO DE DISEÑO E INNOVACIÓN
        TECNOLOGÍCA INDUSTRIAL-Regional Risaralda<br />
        Dirección: Transv. 7 Calle 26 Barrio Santa Isabel Dosquebradas - +57 (6)
        3113700 IP 63361<br />
        Conmutador Nacional (57 1) 5461500 <br /> Atención telefónica: lunes a
        viernes 8:00 a.m. a 6:00 p.m. <br />
        Atención al ciudadano: Bogotá (57 1) 3430111 - Línea gratuita y resto del
        país 018000 910270 <br />
        Atención al empresario: Bogotá (57 1) 3430101 - Línea gratuita y resto del
        país 018000 910682
      </p>
      <div class="flex bg-white justify-between rounded-xl p-4 w-fit h-fit gap-3">
        <img class="h-14" src={logoSena} alt="Logo de SENA" />
        <hr class="bg-primary h-auto rounded-xl" style="width: 2px;">
        <img class="h-14" src={logoSennova} alt="" />
      </div>
    </div>
  </footer>
</div>
