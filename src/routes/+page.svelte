<script>
  import { onMount } from "svelte";
  import playerBanner from "$lib/images/player-banner.webp";
  import Post from "../components/Post.svelte";
  import RadioPlayer from "../components/RadioPlayer.svelte";
  import { RADIO_URL } from "$lib/js/config";
</script>

<svelte:head>
  <title>CDITI RADIO - Inicio</title>
  <meta
    name="description"
    content="Radio en vivo del Centro de Diseño e Innovación Tecnológica Industrial SENA"
  />
</svelte:head>

<main class="max-w-7xl px-4 py-5 mx-auto">
  <div class="w-full bg-white rounded-xl overflow-hidden shadow-md mb-12">
    <!-- Banner con gradiente de la paleta de colores -->
    <div
      class="w-full h-60 bg-cover bg-center relative"
      style="background-image: url({playerBanner});"
    >
      <div
        class="w-full h-full backdrop-blur-[2px] bg-gradient-to-br from-secondary/80 via-secondary/70 to-secondary/60 flex flex-col justify-center items-center p-6"
      >
        <!-- Elemento decorativo con gradiente -->
        <div
          class="absolute top-0 left-0 right-0 h-1 bg-gradient-to-r from-primary via-tertiary to-alternative"
        ></div>

        <h1
          class="text-white text-3xl md:text-4xl font-bold mb-2 text-center relative"
        >
          CDITI RADIO
          <span
            class="absolute -bottom-1 left-0 right-0 h-0.5 bg-tertiary/80 transform translate-y-1 rounded-full"
          ></span>
        </h1>
        <p class="text-white text-lg md:text-xl text-center max-w-2xl">
          La voz del Centro de Diseño e Innovación Tecnológica Industrial SENA
        </p>
      </div>
    </div>

    <div
      class="p-6 flex flex-col md:flex-row gap-6 items-center justify-between"
    >
      <div class="w-full md:w-1/2">
        <h2 class="text-2xl font-semibold text-secondary mb-3">
          Escúchanos en vivo
        </h2>
        <p class="text-secondary/80 mb-4">
          Disfruta de nuestra programación variada con contenido educativo,
          informativo y entretenimiento.
        </p>
        <div class="flex gap-4 items-center mb-2">
          <a
            href="/radio"
            class="text-primary hover:text-tertiary transition-colors font-medium flex items-center"
          >
            Ver programación completa
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-4 w-4 ml-1"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
              <path
                fill-rule="evenodd"
                d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z"
                clip-rule="evenodd"
              />
            </svg>
          </a>
        </div>
      </div>
      <iframe
        src="https://owncast.selftcode.com/embed/video"
        title="Owncast"
        referrerpolicy="origin"
        allowfullscreen
        class="w-full w-full aspect-video rounded-xl"
      >
      </iframe>
      <div class="w-full md:w-1/2 flex justify-center">
        <RadioPlayer streamUrl={RADIO_URL} stationName="CDITI Radio" />
      </div>
    </div>

    <div class="px-6 pb-6">
      <div
        class="w-full p-5 bg-alternative/5 rounded-lg border border-alternative/10"
      >
        <h3 class="text-lg font-medium text-secondary mb-3">
          Síguenos en redes sociales
        </h3>
        <div class="flex gap-6 justify-center md:justify-start flex-wrap">
          <a
            href="#"
            class="text-2xl hover:text-primary transition-colors flex flex-col items-center"
          >
            <svg
              class="h-7 w-7"
              fill="currentColor"
              viewBox="0 0 24 24"
              aria-hidden="true"
            >
              <path
                fill-rule="evenodd"
                d="M22 12c0-5.523-4.477-10-10-10S2 6.477 2 12c0 4.991 3.657 9.128 8.438 9.878v-6.987h-2.54V12h2.54V9.797c0-2.506 1.492-3.89 3.777-3.89 1.094 0 2.238.195 2.238.195v2.46h-1.26c-1.243 0-1.63.771-1.63 1.562V12h2.773l-.443 2.89h-2.33v6.988C18.343 21.128 22 16.991 22 12z"
                clip-rule="evenodd"
              />
            </svg>
            <span class="text-xs mt-1 text-secondary/80">Facebook</span>
          </a>
          <a
            href="#"
            class="text-2xl hover:text-tertiary transition-colors flex flex-col items-center"
          >
            <svg
              class="h-7 w-7"
              fill="currentColor"
              viewBox="0 0 24 24"
              aria-hidden="true"
            >
              <path
                fill-rule="evenodd"
                d="M12.315 2c2.43 0 2.784.013 3.808.06 1.064.049 1.791.218 2.427.465a4.902 4.902 0 011.772 1.153 4.902 4.902 0 011.153 1.772c.247.636.416 1.363.465 2.427.048 1.067.06 1.407.06 4.123v.08c0 2.643-.012 2.987-.06 4.043-.049 1.064-.218 1.791-.465 2.427a4.902 4.902 0 01-1.153 1.772 4.902 4.902 0 01-1.772 1.153c-.636.247-1.363.416-2.427.465-1.067.048-1.407.06-4.123.06h-.08c-2.643 0-2.987-.012-4.043-.06-1.064-.049-1.791-.218-2.427-.465a4.902 4.902 0 01-1.772-1.153 4.902 4.902 0 01-1.153-1.772c-.247-.636-.416-1.363-.465-2.427-.047-1.024-.06-1.379-.06-3.808v-.63c0-2.43.013-2.784.06-3.808.049-1.064.218-1.791.465-2.427a4.902 4.902 0 011.153-1.772A4.902 4.902 0 015.45 2.525c.636-.247 1.363-.416 2.427-.465C8.901 2.013 9.256 2 11.685 2h.63zm-.081 1.802h-.468c-2.456 0-2.784.011-3.807.058-.975.045-1.504.207-1.857.344-.467.182-.8.398-1.15.748-.35.35-.566.683-.748 1.15-.137.353-.3.882-.344 1.857-.047 1.023-.058 1.351-.058 3.807v.468c0 2.456.011 2.784.058 3.807.045.975.207 1.504.344 1.857.182.466.399.8.748 1.15.35.35.683.566 1.15.748.353.137.882.3 1.857.344 1.054.048 1.37.058 4.041.058h.08c2.597 0 2.917-.01 3.96-.058.976-.045 1.505-.207 1.858-.344.466-.182.8-.398 1.15-.748.35-.35.566-.683.748-1.15.137-.353.3-.882.344-1.857.048-1.055.058-1.37.058-4.041v-.08c0-2.597-.01-2.917-.058-3.96-.045-.976-.207-1.505-.344-1.858a3.097 3.097 0 00-.748-1.15 3.098 3.098 0 00-1.15-.748c-.353-.137-.882-.3-1.857-.344-1.023-.047-1.351-.058-3.807-.058zM12 6.865a5.135 5.135 0 110 10.27 5.135 5.135 0 010-10.27zm0 1.802a3.333 3.333 0 100 6.666 3.333 3.333 0 000-6.666zm5.338-3.205a1.2 1.2 0 110 2.4 1.2 1.2 0 010-2.4z"
                clip-rule="evenodd"
              />
            </svg>
            <span class="text-xs mt-1 text-secondary/80">Instagram</span>
          </a>
          <a
            href="#"
            class="text-2xl hover:text-alternative transition-colors flex flex-col items-center"
          >
            <svg
              class="h-7 w-7"
              fill="currentColor"
              viewBox="0 0 24 24"
              aria-hidden="true"
            >
              <path
                d="M8.29 20.251c7.547 0 11.675-6.253 11.675-11.675 0-.178 0-.355-.012-.53A8.348 8.348 0 0022 5.92a8.19 8.19 0 01-2.357.646 4.118 4.118 0 001.804-2.27 8.224 8.224 0 01-2.605.996 4.107 4.107 0 00-6.993 3.743 11.65 11.65 0 01-8.457-4.287 4.106 4.106 0 001.27 5.477A4.072 4.072 0 012.8 9.713v.052a4.105 4.105 0 003.292 4.022 4.095 4.095 0 01-1.853.07 4.108 4.108 0 003.834 2.85A8.233 8.233 0 012 18.407a11.616 11.616 0 006.29 1.84"
              />
            </svg>
            <span class="text-xs mt-1 text-secondary/80">Twitter</span>
          </a>
        </div>
      </div>
    </div>
  </div>

  <!-- Sección de Blog (Posts de Blogspot) -->
  <div
    class="bg-white rounded-xl shadow-md overflow-hidden border border-tertiary/10 p-6 mb-10"
  >
    <div class="flex items-center justify-between mb-6">
      <h2 class="text-2xl font-bold text-secondary relative inline-block">
        Blog del CDITI
        <span class="absolute -bottom-1 left-0 right-0 h-0.5 bg-tertiary"
        ></span>
      </h2>
      <a
        href="https://senarisaraldadosquebradas.blogspot.com/"
        target="_blank"
        rel="noopener noreferrer"
        class="flex items-center text-primary hover:text-tertiary text-sm font-medium transition-colors"
      >
        Visitar blog completo
        <svg
          class="w-3.5 h-3.5 ms-1.5"
          aria-hidden="true"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 18 18"
        >
          <path
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M15 11v4.833A1.166 1.166 0 0 1 13.833 17H2.167A1.167 1.167 0 0 1 1 15.833V4.167A1.166 1.166 0 0 1 2.167 3h4.618m4.447-2H17v5.768M9.111 8.889l7.778-7.778"
          />
        </svg>
      </a>
    </div>

    <!-- Agregamos aquí el componente Post reformateado para adaptarse a nuestro estilo -->
    <div class="post-wrapper">
      <Post />
    </div>
  </div>

  <!-- Sección de contenido destacado -->
  <div class="mb-10">
    <div class="flex items-center justify-between mb-6">
      <h2 class="text-2xl font-bold text-secondary relative inline-block">
        Contenido destacado
        <span class="absolute -bottom-1 left-0 right-0 h-0.5 bg-tertiary/80"
        ></span>
      </h2>
      <a
        href="#"
        class="text-primary hover:text-tertiary text-sm font-medium transition-colors"
        >Ver todo</a
      >
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <!-- Tarjetas de contenido -->
      <div
        class="bg-white rounded-xl shadow-md overflow-hidden border border-tertiary/10 hover:shadow-lg transition-shadow"
      >
        <div class="h-40 bg-tertiary/10"></div>
        <div class="p-5">
          <span
            class="inline-block px-2 py-1 text-xs font-medium text-white bg-primary rounded-full mb-2"
            >Entrevista</span
          >
          <h3 class="text-lg font-medium text-secondary mb-2">
            Innovación tecnológica en el SENA
          </h3>
          <p class="text-sm text-secondary/70 mb-3">
            Conversando con los líderes de proyectos tecnológicos en el CDITI.
          </p>
          <div class="flex justify-between items-center">
            <span class="text-xs text-secondary/60">15 mayo, 2024</span>
            <a
              href="#"
              class="text-primary hover:text-tertiary text-sm font-medium transition-colors"
              >Leer más</a
            >
          </div>
        </div>
      </div>

      <div
        class="bg-white rounded-xl shadow-md overflow-hidden border border-alternative/10 hover:shadow-lg transition-shadow"
      >
        <div class="h-40 bg-alternative/10"></div>
        <div class="p-5">
          <span
            class="inline-block px-2 py-1 text-xs font-medium text-white bg-alternative rounded-full mb-2"
            >Podcast</span
          >
          <h3 class="text-lg font-medium text-secondary mb-2">
            El futuro de la educación técnica
          </h3>
          <p class="text-sm text-secondary/70 mb-3">
            Analizando las tendencias educativas en formación técnica y
            tecnológica.
          </p>
          <div class="flex justify-between items-center">
            <span class="text-xs text-secondary/60">8 mayo, 2024</span>
            <a
              href="#"
              class="text-primary hover:text-tertiary text-sm font-medium transition-colors"
              >Escuchar</a
            >
          </div>
        </div>
      </div>

      <div
        class="bg-white rounded-xl shadow-md overflow-hidden border border-tertiary/10 hover:shadow-lg transition-shadow"
      >
        <div class="h-40 bg-tertiary/10"></div>
        <div class="p-5">
          <span
            class="inline-block px-2 py-1 text-xs font-medium text-white bg-tertiary rounded-full mb-2"
            >Noticia</span
          >
          <h3 class="text-lg font-medium text-secondary mb-2">
            Nuevos proyectos de SENNOVA
          </h3>
          <p class="text-sm text-secondary/70 mb-3">
            Conoce las iniciativas de investigación aplicada que se desarrollan
            en el CDITI.
          </p>
          <div class="flex justify-between items-center">
            <span class="text-xs text-secondary/60">2 mayo, 2024</span>
            <a
              href="#"
              class="text-primary hover:text-tertiary text-sm font-medium transition-colors"
              >Leer más</a
            >
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Sección de agenda o próximos eventos -->
  <div
    class="bg-white rounded-xl shadow-md overflow-hidden border border-secondary/10 p-6 mb-10"
  >
    <h2 class="text-2xl font-bold text-secondary mb-6 relative inline-block">
      Próximos eventos
      <span class="absolute -bottom-1 left-0 right-0 h-0.5 bg-primary/80"
      ></span>
    </h2>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
      <div class="flex items-start">
        <div
          class="flex-shrink-0 mr-4 bg-secondary/5 p-2 rounded-lg border border-secondary/10 text-center"
        >
          <span class="block text-tertiary font-bold text-lg">25</span>
          <span class="block text-secondary/70 text-xs">Mayo</span>
        </div>
        <div>
          <h3 class="text-lg font-medium text-secondary">
            Transmisión especial: Día del Aprendiz
          </h3>
          <p class="text-sm text-secondary/80">
            Programación especial celebrando los talentos de nuestros
            aprendices.
          </p>
          <div class="flex items-center mt-2 text-xs text-secondary/70">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-4 w-4 mr-1"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
            10:00 AM - 2:00 PM
          </div>
        </div>
      </div>

      <div class="flex items-start">
        <div
          class="flex-shrink-0 mr-4 bg-secondary/5 p-2 rounded-lg border border-secondary/10 text-center"
        >
          <span class="block text-alternative font-bold text-lg">30</span>
          <span class="block text-secondary/70 text-xs">Mayo</span>
        </div>
        <div>
          <h3 class="text-lg font-medium text-secondary">
            Entrevista: Innovación en Risaralda
          </h3>
          <p class="text-sm text-secondary/80">
            Conversación con líderes del ecosistema de innovación regional.
          </p>
          <div class="flex items-center mt-2 text-xs text-secondary/70">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-4 w-4 mr-1"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
            3:00 PM - 4:30 PM
          </div>
        </div>
      </div>
    </div>

    <div class="mt-6 text-center">
      <a
        href="#"
        class="inline-block px-5 py-2 bg-secondary text-white font-medium rounded-lg hover:bg-secondary/90 transition-colors"
      >
        Ver calendario completo
      </a>
    </div>
  </div>
</main>

<style>
  /* Estilos específicos para el componente Post dentro de la página principal */
  .post-wrapper :global(.mb-10) {
    margin-bottom: 0 !important;
  }

  .post-wrapper :global(.text-center.mb-8) {
    display: none; /* Ocultamos el título del componente Post ya que tenemos nuestro propio título */
  }
</style>
