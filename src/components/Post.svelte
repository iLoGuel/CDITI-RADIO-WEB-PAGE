<!-- src/lib/components/Post.svelte -->
<script>
  import { onMount } from "svelte";
  import { API_URL, PLACEHOLDER_IMAGE, ERROR_IMAGE } from "$lib/js/config";

  class Post {
    constructor({ title, url, content, published }) {
      this.title = title;
      this.url = url;
      this.content = content;
      this.publishedDate = published;
    }

    extractFirstImage() {
      const parser = new DOMParser();
      const doc = parser.parseFromString(this.content, "text/html");
      const imgElement = doc.querySelector("img");
      return imgElement ? imgElement.src : PLACEHOLDER_IMAGE;
    }

    static handleImageError(event) {
      event.target.src = ERROR_IMAGE;
    }
  }

  let posts = [];
  let loading = true;
  // Función para obtener los posts
  async function fetchPosts() {
    try {
      const response = await fetch(API_URL);
      const data = await response.json();
      posts = data.items.map((item) => new Post(item));
    } catch (error) {
      console.error("Error al obtener los posts:", error);
    } finally {
      loading = false;
    }
  }

  // Función para formatear la fecha en español
  function formatDate(dateString) {
    const date = new Date(dateString);
    const options = { year: "numeric", month: "long", day: "numeric" };
    return date.toLocaleDateString("es-ES", options);
  }

  onMount(() => {
    fetchPosts();
  });
</script>

<h1 class="text-center text-xl font-bold my-5">Últimas Publicaciones</h1>
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
  {#if loading}
    <!-- Esqueleto de carga -->
    {#each Array(8) as _, i}
      <div
        class="curso rounded-xl hover:scale-105 shadow-sm bg-gray-100 transition-all animate-pulse"
      >
        <div class="w-full h-48 bg-gray-200 rounded-t-xl animate-pulse"></div>
        <div class="p-2">
          <div
            class="font-semibold line-clamp-2 bg-gray-200 h-6 w-4/5 mb-2 animate-pulse"
          ></div>
          <div class="bg-gray-200 h-4 w-2/3 animate-pulse"></div>
        </div>
      </div>
    {/each}
  {:else}
    <!-- Renderizar los posts -->
    {#each posts as post}
      <div
        class="curso rounded-xl shadow-sm hover:scale-105 bg-gray-100 transition-all flex flex-col"
      >
        {#if post.content && post.extractFirstImage()}
          <img
            src={post.extractFirstImage()}
            alt={post.title}
            class="w-full h-48 object-cover bg-gray-100 rounded-t-xl"
            on:error={post.handleImageError}
          />
        {:else}
          <div class="w-full h-48 bg-gray-200 rounded-t-xl animate-pulse"></div>
          <!-- Esqueleto de carga -->
        {/if}
        <div class="p-3 flex flex-col justify-between gap-3 h-full">
          <div class="flex flex-col gap-1">
            <span class="text-gray-500 text-sm"
              ><i class="fa-regular fa-clock"></i>
              {formatDate(post.publishedDate)}</span
            >
            <h2 class="font-semibold line-clamp-2">{post.title}</h2>
          </div>
          <a
            href={post.url}
            target="_blank"
            class=" text-sm bg-secondary shadow-sm text-primary px-3 py-2 rounded-xl w-fit"
            >Leer más...</a
          >
        </div>
      </div>
    {/each}
  {/if}
</div>
