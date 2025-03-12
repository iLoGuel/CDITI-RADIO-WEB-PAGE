<script>
  import RadioPlayer from '../../components/RadioPlayer.svelte';
</script>

<svelte:head>
  <title>Radio en vivo | CDITI Radio</title>
  <meta name="description" content="Escucha CDITI Radio en vivo - La radio del Centro de Diseño e Innovación Tecnológica Industrial - SENA Regional Risaralda" />
</svelte:head>

<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
  <div class="text-center mb-12">
    <h1 class="text-3xl md:text-4xl font-bold text-secondary mb-4">CDITI Radio En Vivo</h1>
    <p class="text-gray-600 max-w-2xl mx-auto">
      Escucha nuestra programación en vivo con contenido actualizado, entrevistas, noticias y eventos del Centro de Diseño e Innovación Tecnológica Industrial.
    </p>
  </div>
  
  <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-start">
    <!-- Reproductor y Descripción -->
    <div class="order-2 lg:order-1 flex flex-col items-center">
      <div class="mb-8 w-full max-w-md mx-auto">
        <RadioPlayer 
          showMiniPlayer={false} 
          stationName="CDITI Radio" 
          streamUrl="https://streaming.radio.co/example-station" 
        />
      </div>
      
      <div class="bg-white rounded-xl shadow-sm border border-gray-100 p-6 w-full max-w-md">
        <h2 class="text-lg font-semibold text-secondary mb-4">Sobre nuestra radio</h2>
        <p class="text-gray-600 text-sm mb-4">
          CDITI Radio es la emisora oficial del Centro de Diseño e Innovación Tecnológica Industrial del SENA Regional Risaralda. Ofrecemos una programación variada con enfoque educativo, tecnológico y cultural.
        </p>
        <p class="text-gray-600 text-sm">
          Nuestra misión es mantener informada a la comunidad SENA y al público general sobre las actividades, proyectos y logros de nuestros aprendices e instructores, además de ofrecer contenido educativo de calidad.
        </p>
      </div>
    </div>
    
    <!-- Programación -->
    <div class="order-1 lg:order-2">
      <div class="bg-white rounded-xl shadow-sm border border-gray-100 p-6">
        <h2 class="text-lg font-semibold text-secondary mb-6 flex items-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
          </svg>
          Programación Semanal
        </h2>
        
        <div class="space-y-5">
          <!-- Lunes a Viernes -->
          <div>
            <h3 class="text-primary font-medium mb-2">Lunes a Viernes</h3>
            <ul class="space-y-3">
              <li class="flex">
                <span class="text-gray-500 w-24 text-sm">7:00 - 9:00</span>
                <div>
                  <span class="text-gray-800 font-medium text-sm">Despierta CDITI</span>
                  <p class="text-gray-500 text-xs">Actualidad, música y noticias para comenzar el día</p>
                </div>
              </li>
              <li class="flex">
                <span class="text-gray-500 w-24 text-sm">10:00 - 12:00</span>
                <div>
                  <span class="text-gray-800 font-medium text-sm">Innovación Tecnológica</span>
                  <p class="text-gray-500 text-xs">Últimas tendencias en tecnología y innovación</p>
                </div>
              </li>
              <li class="flex">
                <span class="text-gray-500 w-24 text-sm">14:00 - 16:00</span>
                <div>
                  <span class="text-gray-800 font-medium text-sm">Entrevistas SENA</span>
                  <p class="text-gray-500 text-xs">Conversaciones con instructores, aprendices y expertos</p>
                </div>
              </li>
            </ul>
          </div>
          
          <!-- Sábados -->
          <div>
            <h3 class="text-primary font-medium mb-2">Sábados</h3>
            <ul class="space-y-3">
              <li class="flex">
                <span class="text-gray-500 w-24 text-sm">9:00 - 11:00</span>
                <div>
                  <span class="text-gray-800 font-medium text-sm">Proyectos Destacados</span>
                  <p class="text-gray-500 text-xs">Presentación de los mejores proyectos de nuestros aprendices</p>
                </div>
              </li>
              <li class="flex">
                <span class="text-gray-500 w-24 text-sm">13:00 - 15:00</span>
                <div>
                  <span class="text-gray-800 font-medium text-sm">Cultura y Región</span>
                  <p class="text-gray-500 text-xs">Lo mejor de la cultura en Risaralda</p>
                </div>
              </li>
            </ul>
          </div>
        </div>
        
        <div class="mt-6 pt-6 border-t border-gray-100">
          <a href="/contacto" class="inline-flex items-center text-sm font-medium text-primary hover:text-primary/80 transition-colors">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
            ¿Preguntas sobre nuestra programación? Contáctanos
          </a>
        </div>
      </div>
    </div>
  </div>
  
  <!-- Llamada a la acción -->
  <div class="mt-16 bg-primary/10 rounded-xl p-8 text-center">
    <h2 class="text-xl font-bold text-secondary mb-4">¿Tienes un programa que quieres compartir?</h2>
    <p class="text-gray-600 mb-6 max-w-xl mx-auto">
      Si eres instructor o aprendiz del SENA y tienes una idea para un programa de radio, nos encantaría escucharte.
    </p>
    <a 
      href="/contacto" 
      class="inline-flex items-center px-5 py-2.5 bg-primary hover:bg-primary/90 text-white font-medium rounded-xl transition-all hover:scale-105"
    >
      Propón tu programa
    </a>
  </div>
</div> 