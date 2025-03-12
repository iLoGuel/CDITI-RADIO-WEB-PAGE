<script>
  import { onMount } from "svelte";
  import playerBanner from "$lib/images/player-banner.webp";
  import Post from "../components/Post.svelte";
  import RadioPlayer from "../components/RadioPlayer.svelte";
  import { RADIO_URL } from "$lib/js/config";
</script>

<svelte:head>
  <title>CDITI RADIO - Inicio</title>
  <meta name="description" content="Radio en vivo del Centro de Diseño e Innovación Tecnológica Industrial SENA" />
</svelte:head>

<main class="max-w-7xl px-4 py-5 mx-auto">
  <div
    class="w-full bg-white rounded-xl overflow-hidden shadow-md mb-8"
  >
    <div 
      class="w-full h-60 bg-cover bg-center" 
      style="background-image: url({playerBanner});"
    >
      <div class="w-full h-full backdrop-blur-[2px] bg-black/20 flex flex-col justify-center items-center p-6">
        <h1 class="text-white text-3xl md:text-4xl font-bold mb-2 text-center">CDITI RADIO</h1>
        <p class="text-white text-lg md:text-xl text-center max-w-2xl">
          La voz del Centro de Diseño e Innovación Tecnológica Industrial SENA
        </p>
      </div>
    </div>
    
    <div class="p-6 flex flex-col md:flex-row gap-6 items-center justify-between">
      <div class="w-full md:w-1/2">
        <h2 class="text-2xl font-semibold text-secondary mb-3">Escúchanos en vivo</h2>
        <p class="text-gray-600 mb-4">
          Disfruta de nuestra programación variada con contenido educativo, informativo y entretenimiento.
        </p>
        <div class="flex gap-4 items-center mb-2">
          <a href="/radio" class="text-primary hover:underline font-medium">Ver programación completa →</a>
        </div>
      </div>
      
      <div class="w-full md:w-1/2 flex justify-center">
        <RadioPlayer streamUrl={RADIO_URL} stationName="CDITI Radio" />
      </div>
    </div>
    
    <div class="px-6 pb-6">
      <div class="w-full p-4 bg-primary/5 rounded-lg border border-primary/10">
        <h3 class="text-lg font-medium text-secondary mb-2">Síguenos en redes sociales</h3>
        <div class="flex gap-6 justify-center md:justify-start flex-wrap">
          <a href="#" class="text-2xl text-primary hover:text-tertiary transition-colors">
            <i class="fa-brands fa-facebook"></i>
          </a>
          <a href="#" class="text-2xl text-primary hover:text-tertiary transition-colors">
            <i class="fa-brands fa-instagram"></i>
          </a>
          <a href="#" class="text-2xl text-primary hover:text-tertiary transition-colors">
            <i class="fa-brands fa-tiktok"></i>
          </a>
          <a href="#" class="text-2xl text-primary hover:text-tertiary transition-colors">
            <i class="fa-brands fa-youtube"></i>
          </a>
          <a href="#" class="text-2xl text-primary hover:text-tertiary transition-colors">
            <i class="fa-brands fa-spotify"></i>
          </a>
        </div>
      </div>
    </div>
  </div>
  
  <hr class="my-8" />
  
  <!-- Muestra el componente Post para blog o noticias -->
  <Post />
</main>