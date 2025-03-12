<script>
  import RadioPlayer from './RadioPlayer.svelte';
</script>

<section class="bg-gradient-to-br from-white to-gray-50 py-12 rounded-2xl border border-gray-100 shadow-sm">
  <div class="container mx-auto px-4">
    <div class="flex flex-col lg:flex-row items-center justify-between gap-8 lg:gap-12">
      <!-- Información de la radio -->
      <div class="max-w-xl">
        <div class="flex items-center mb-2">
          <div class="relative flex h-3 w-3 mr-2">
            <span class="absolute inline-flex h-full w-full rounded-full bg-tertiary opacity-75 animate-ping"></span>
            <span class="relative inline-flex rounded-full h-3 w-3 bg-tertiary"></span>
          </div>
          <span class="text-sm font-medium text-primary">En vivo ahora</span>
        </div>
        
        <h2 class="text-2xl md:text-3xl font-bold text-secondary mb-4">CDITI Radio</h2>
        <p class="text-gray-600 mb-6">
          Escucha nuestra programación en vivo con contenido exclusivo del Centro de Diseño e Innovación Tecnológica Industrial. Mantente conectado con las últimas noticias, entrevistas y eventos del SENA Regional Risaralda.
        </p>
        
        <div class="flex items-center gap-4">
          <a 
            href="/radio" 
            class="inline-flex items-center px-5 py-2.5 bg-primary text-white font-medium rounded-xl hover:bg-primary/90 transition-all hover:scale-105"
          >
            Ver programación
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 ml-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
            </svg>
          </a>
          
          <a 
            href="https://senarisaraldadosquebradas.blogspot.com/"
            target="_blank" 
            rel="noopener noreferrer"
            class="inline-flex items-center px-4 py-2 text-gray-700 font-medium rounded-xl border border-gray-200 hover:bg-gray-50 transition-all hover:border-primary/30"
          >
            Blog del CDITI
            <svg class="w-3.5 h-3.5 ms-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 18 18">
              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11v4.833A1.166 1.166 0 0 1 13.833 17H2.167A1.167 1.167 0 0 1 1 15.833V4.167A1.166 1.166 0 0 1 2.167 3h4.618m4.447-2H17v5.768M9.111 8.889l7.778-7.778"/>
            </svg>
          </a>
        </div>
      </div>
      
      <!-- Reproductor de Radio -->
      <div class="w-full max-w-md lg:mt-0 mt-8">
        <div class="bg-white p-6 rounded-xl shadow-md border border-gray-100">
          <RadioPlayer 
            showMiniPlayer={false} 
            stationName="CDITI Radio" 
            streamUrl="https://streaming.radio.co/example-station" 
          />
          
          <div class="mt-5 pt-5 border-t border-gray-100">
            <h3 class="text-sm font-medium text-secondary mb-3">Próximos programas</h3>
            <ul class="space-y-3">
              <li class="flex items-center text-sm">
                <span class="bg-primary/10 text-primary text-xs font-medium py-0.5 px-2 rounded mr-3 min-w-[72px] text-center">
                  7:00 - 9:00
                </span>
                <span class="text-gray-700">Despierta CDITI</span>
              </li>
              <li class="flex items-center text-sm">
                <span class="bg-primary/10 text-primary text-xs font-medium py-0.5 px-2 rounded mr-3 min-w-[72px] text-center">
                  10:00 - 12:00
                </span>
                <span class="text-gray-700">Innovación Tecnológica</span>
              </li>
            </ul>
            
            <a href="/radio" class="text-primary hover:text-primary/80 text-sm font-medium inline-flex items-center mt-4">
              <span>Ver toda la programación</span>
              <svg xmlns="http://www.w3.org/2000/svg" class="h-3.5 w-3.5 ml-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
              </svg>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</section> 