<script>
  import { onMount, onDestroy } from "svelte";
  import playerBanner from "$lib/images/player-banner.webp";
  import { RADIO_URL } from "$lib/js/config";
  import Post from "../components/Post.svelte";

  // Inicializa la variable `playing` como un booleano, establecida en false
  let playing = false;

  /**
   * @type {HTMLAudioElement}
   */
  let player;

  onMount(() => {
    // Inicializa la variable `player` como un nuevo objeto Audio
    player = new Audio();
  });

  /**
   * Reproduce o pausa la transmisión de radio
   */
  function playRadio() {
    if (playing) {
      // Si la radio está actualmente reproduciendo, detén la reproducción y actualiza la variable `playing`
      player.src = '';
      playing =!playing;
    } else {
      // Si la radio no está actualmente reproduciendo, establece la fuente `player` a `RADIO_URL` y reproduce la transmisión
      player.src = RADIO_URL;
      player.play();
      playing =!playing;
    }
  }

  onDestroy(() => {
    // Si el componente se está destruyendo y la radio está actualmente reproduciendo, detén la reproducción
    if (playing) {
      playRadio();
    }
  });
</script>

<svelte:head>
  <title>CDITI RADIO - Inicio</title>
  <meta name="description" content="CDITI RADIO" />
</svelte:head>

<main class="max-w-screen-xl px-4 py-5 mx-auto">
  <div
    class="w-full h-80 bg-white flex items-end rounded-xl overflow-hidden shadow bg-cover"
    style="background-image: url({playerBanner});"
  >
    <div
      class="w-full backdrop-blur-sm backdrop-brightness-50 flex px-6 py-4 text-white text-2xl justify-between flex-wrap gap-6"
    >
      <button on:click={playRadio} class="transition-all hover:scale-105">
        <!-- Utiliza la clase `fa-solid` para mostrar el icono apropiado basado en la variable `playing` -->
        <i class={`fa-solid ${playing? 'fa-stop' : 'fa-play'}`}></i>
        <!-- Muestra el texto apropiado basado en la variable `playing` -->
        {playing? 'Detener' : 'Reproducir'}
      </button>
      <div class="flex gap-4 items-center">
        <!-- Agrega iconos de redes sociales con enlaces -->
        <i
          class="fa-brands fa-facebook transition-all hover:scale-105 cursor-pointer"
        ></i>
        <i
          class="fa-brands fa-instagram transition-all hover:scale-105 cursor-pointer"
        ></i>
        <i
          class="fa-brands fa-tiktok transition-all hover:scale-105 cursor-pointer"
        ></i>
        <i
          class="fa-brands fa-youtube transition-all hover:scale-105 cursor-pointer"
        ></i>
        <i
          class="fa-brands fa-spotify transition-all hover:scale-105 cursor-pointer"
        ></i>
      </div>
    </div>
  </div>
  <hr class="my-5" />
  <!-- Muestra el componente `Post` -->
  <Post />
</main>