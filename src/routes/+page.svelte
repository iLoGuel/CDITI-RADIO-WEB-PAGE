<script>
  import { onMount } from "svelte";

  // URL de la API de Blogger
  const apiUrl =
    "https://www.googleapis.com/blogger/v3/blogs/2909351373174179842/posts/?%20fields=items(title%2Curl%2Ccontent%2Cpublished)&key=AIzaSyDQtvqyPNc5IUXvEpmj9E7vx_4V2dh3mdw";

  class Post {
    constructor(title, url, content, publishedDate) {
      this.title = title;
      this.url = url;
      this.content = content;
      this.publishedDate = publishedDate;
    }

    extractFirstImage() {
      const parser = new DOMParser();
      const doc = parser.parseFromString(this.content, "text/html");
      const imgElement = doc.querySelector("img");
      return imgElement? imgElement.src : "https://placehold.co/600x400?text=Sin+imagen"; // Usar un placeholder si no se encuentra una imagen
    }

    handleImageError(event) {
      event.target.src = "https://placehold.co/600x400?text=error"; // Usar un placeholder en caso de error
    }
  }

  let posts = [];
  let loading = true;

  // Función para obtener los posts
  async function fetchPosts() {
    try {
      const response = await fetch(apiUrl);
      const data = await response.json();
      posts = data.items.map((item) => new Post(item.title, item.url, item.content, item.published));
      loading = false;
    } catch (error) {
      console.error("Error al obtener los posts:", error);
    }
  }

  onMount(fetchPosts);
</script>

<svelte:head>
  <title>CDITI RADIO - Inicio</title>
  <meta name="description" content="CDITI RADIO" />
</svelte:head>

<main class="max-w-screen-xl px-4 py-5 mx-auto">
  <!-- svelte-ignore a11y-missing-attribute -->
  <iframe
    class="rounded-xl shadow"
    src="https://zeno.fm/player/cditi-radio-sena"
    width="100%"
    height="300"
    frameborder="0"
    scrolling="no"
  ></iframe>
  <hr class="my-5" />
  <h1 class="text-center text-xl font-bold my-5">Ultimas Publicaciones</h1>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
    {#if loading}
      <!-- Esqueleto de carga -->
      {#each Array(8) as _, i}
        <div class="curso rounded-xl hover:scale-105 shadow bg-gray-100 transition-all animate-pulse">
          <div class="w-full h-48 bg-gray-200 rounded-t-xl"></div>
          <div class="p-2">
            <div class="font-semibold line-clamp-2 bg-gray-200 h-6 w-4/5 mb-2"></div>
            <div class="bg-gray-200 h-4 w-2/3"></div>
          </div>
        </div>
      {/each}
    {:else}
      <!-- Renderizar los posts -->
      {#each posts as post}
        <div class="curso rounded-xl shadow hover:scale-105 bg-gray-100 transition-all">
          {#if post.content && post.extractFirstImage()}
            <img
              src={post.extractFirstImage()}
              alt={post.title}
              class="w-full h-48 object-cover bg-gray-100 rounded-t-xl"
              on:error={post.handleImageError}
            />
          {:else}
            <div class="w-full h-48 bg-gray-200 rounded-t-xl"></div> <!-- Esqueleto de carga -->
          {/if}
          <div class="p-2">
            <h2 class="font-semibold line-clamp-2">{post.title}</h2>
            <a href={post.url} target="_blank" class="text-blue-500">Leer más...</a>
          </div>
        </div>
      {/each}
    {/if}
  </div>
</main>
