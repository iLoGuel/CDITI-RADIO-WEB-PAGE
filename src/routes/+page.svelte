<script>
  import { onMount, onDestroy} from "svelte";
  import playerBanner from "$lib/images/player-banner.webp";
  import { RADIO_URL } from "$lib/js/config";
  import Post from "../components/Post.svelte";

  let playing = false;
  /**
   * @type {HTMLAudioElement}
   */
  let player;

  onMount(() => {
    player = new Audio();
  });

  function playRadio() {
  if (playing) {
    player.src = '';
    playing =!playing;
  } else {
    player.src = RADIO_URL;
    player.play();
    playing =!playing;
  }
}

onDestroy(()=>{
  if(playing){
    playRadio();
  }
})

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
        <i class={`fa-solid ${playing? 'fa-stop':'fa-play'}`}></i>
        {playing? 'Detener' : 'Reproducir'}

      </button>
      <div class="flex gap-4 items-center">
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
  <Post />
</main>
