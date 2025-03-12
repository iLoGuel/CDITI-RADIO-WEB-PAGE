<!-- src/lib/components/Post.svelte -->
<script lang="ts">
  import { onMount } from "svelte";
  import { API_URL, PLACEHOLDER_IMAGE, ERROR_IMAGE } from "$lib/js/config";

  interface PostData {
    title: string;
    url: string;
    content: string;
    published: string;
  }

  class Post {
    title: string;
    url: string;
    content: string;
    publishedDate: string;

    constructor({ title, url, content, published }: PostData) {
      this.title = title;
      this.url = url;
      this.content = content;
      this.publishedDate = published;
    }

    extractFirstImage(): string {
      const parser = new DOMParser();
      const doc = parser.parseFromString(this.content, "text/html");
      const imgElement = doc.querySelector("img");
      return imgElement ? imgElement.src : PLACEHOLDER_IMAGE;
    }

    static handleImageError(event: Event): void {
      const target = event.target as HTMLImageElement;
      target.src = ERROR_IMAGE;
    }
  }

  let posts: Post[] = [];
  let loading = true;
  // Función para obtener los posts
  async function fetchPosts(): Promise<void> {
    try {
      const response = await fetch(API_URL);
      const data = await response.json();
      posts = data.items.map((item: PostData) => new Post(item));
    } catch (error) {
      console.error("Error al obtener los posts:", error);
    } finally {
      loading = false;
    }
  }

  // Función para formatear la fecha en español
  function formatDate(dateString: string): string {
    const date = new Date(dateString);
    const options: Intl.DateTimeFormatOptions = { 
      year: "numeric", 
      month: "long", 
      day: "numeric" 
    };
    return date.toLocaleDateString("es-ES", options);
  }

  onMount(() => {
    fetchPosts();
  });
</script>

<div class="mb-10">
  <div class="text-center mb-8">
    <h2 class="text-2xl md:text-3xl font-bold text-secondary mb-3">Últimas Publicaciones</h2>
    <p class="text-gray-600 max-w-2xl mx-auto">
      Mantente informado sobre las últimas noticias y eventos del Centro de Diseño e Innovación Tecnológica Industrial
    </p>
  </div>
  
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
    {#if loading}
      <!-- Esqueleto de carga -->
      {#each Array(6) as _, i}
        <div
          class="bg-white rounded-xl shadow-md border border-gray-100 overflow-hidden hover:shadow-lg transition-all"
        >
          <div class="w-full h-48 bg-gray-200 animate-pulse"></div>
          <div class="p-5">
            <div class="h-4 w-1/3 bg-gray-200 rounded mb-3 animate-pulse"></div>
            <div class="h-6 w-4/5 bg-gray-200 rounded mb-4 animate-pulse"></div>
            <div class="h-4 w-2/3 bg-gray-200 rounded mb-5 animate-pulse"></div>
            <div class="h-9 w-28 bg-gray-200 rounded animate-pulse"></div>
          </div>
        </div>
      {/each}
    {:else}
      <!-- Renderizar los posts -->
      {#each posts as post}
        <div
          class="bg-white rounded-xl shadow-md border border-gray-100 overflow-hidden hover:shadow-lg transition-all flex flex-col h-full"
        >
          {#if post.content && post.extractFirstImage()}
            <div class="relative w-full h-48 overflow-hidden">
              <img
                src={post.extractFirstImage()}
                alt={post.title}
                class="w-full h-full object-cover transition-transform hover:scale-105 duration-500"
                on:error={Post.handleImageError}
              />
            </div>
          {:else}
            <div class="w-full h-48 bg-primary/5 flex items-center justify-center">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16 text-primary/30" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
              </svg>
            </div>
          {/if}
          <div class="p-5 flex flex-col flex-grow">
            <div class="flex items-center mb-3">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-primary mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
              </svg>
              <span class="text-sm text-gray-500">{formatDate(post.publishedDate)}</span>
            </div>
            
            <h3 class="font-semibold text-secondary mb-3 line-clamp-2 text-lg">{post.title}</h3>
            
            <div class="mt-auto pt-4">
              <a
                href={post.url}
                target="_blank"
                rel="noopener noreferrer"
                class="inline-flex items-center px-4 py-2 bg-primary/10 text-primary font-medium rounded-lg hover:bg-primary/20 transition-colors"
              >
                Leer artículo
                <svg class="w-3.5 h-3.5 ms-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 18 18">
                  <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11v4.833A1.166 1.166 0 0 1 13.833 17H2.167A1.167 1.167 0 0 1 1 15.833V4.167A1.166 1.166 0 0 1 2.167 3h4.618m4.447-2H17v5.768M9.111 8.889l7.778-7.778"/>
                </svg>
              </a>
            </div>
          </div>
        </div>
      {/each}
    {/if}
  </div>
</div>
