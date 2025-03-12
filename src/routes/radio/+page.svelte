<script>
  import RadioPlayer from '../../components/RadioPlayer.svelte';
  import { RADIO_URL } from "$lib/js/config";
</script>

<svelte:head>
  <title>Radio en vivo | CDITI Radio</title>
  <meta name="description" content="Escucha CDITI Radio en vivo - La radio del Centro de Diseño e Innovación Tecnológica Industrial - SENA Regional Risaralda" />
</svelte:head>

<main class="max-w-7xl px-4 py-5 mx-auto">
  <div class="w-full bg-white rounded-xl overflow-hidden shadow-md mb-12">
    <!-- Banner con gradiente de la paleta de colores -->
    <div 
      class="w-full h-48 bg-gradient-to-br from-tertiary/20 via-primary/30 to-secondary relative"
    >
      <!-- Ondas de radio animadas como elemento decorativo -->
      <div class="absolute inset-0 overflow-hidden">
        <div class="absolute left-0 right-0 bottom-0 opacity-10">
          <svg viewBox="0 0 1200 120" preserveAspectRatio="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M0,0V46.29c47.79,22.2,103.59,32.17,158,28,70.36-5.37,136.33-33.31,206.8-37.5C438.64,32.43,512.34,53.67,583,72.05c69.27,18,138.3,24.88,209.4,13.08,36.15-6,69.85-17.84,104.45-29.34C989.49,25,1113-14.29,1200,52.47V0Z" opacity=".25" class="fill-white"></path>
            <path d="M0,0V15.81C13,36.92,27.64,56.86,47.69,72.05,99.41,111.27,165,111,224.58,91.58c31.15-10.15,60.09-26.07,89.67-39.8,40.92-19,84.73-46,130.83-49.67,36.26-2.85,70.9,9.42,98.6,31.56,31.77,25.39,62.32,62,103.63,73,40.44,10.79,81.35-6.69,119.13-24.28s75.16-39,116.92-43.05c59.73-5.85,113.28,22.88,168.9,38.84,30.2,8.66,59,6.17,87.09-7.5,22.43-10.89,48-26.93,60.65-49.24V0Z" opacity=".5" class="fill-white"></path>
            <path d="M0,0V5.63C149.93,59,314.09,71.32,475.83,42.57c43-7.64,84.23-20.12,127.61-26.46,59-8.63,112.48,12.24,165.56,35.4C827.93,77.22,886,95.24,951.2,90c86.53-7,172.46-45.71,248.8-84.81V0Z" class="fill-white"></path>
          </svg>
        </div>
      </div>
      
      <!-- Círculos pulsantes simulando ondas de radio -->
      <div class="absolute top-1/2 left-1/3 transform -translate-x-1/2 -translate-y-1/2">
        <div class="w-16 h-16 rounded-full bg-tertiary/10 animate-ping"></div>
      </div>
      <div class="absolute top-1/3 right-1/4 transform -translate-y-1/2">
        <div class="w-12 h-12 rounded-full bg-primary/10 animate-ping" style="animation-delay: 1s;"></div>
      </div>
      
      <div class="w-full h-full flex flex-col justify-center items-center p-6 relative z-10 bg-secondary/80">
        <!-- Elemento decorativo con gradiente -->
        <div class="absolute top-0 left-0 right-0 h-1 bg-gradient-to-r from-primary via-tertiary to-alternative"></div>
        
        <h1 class="text-white text-3xl md:text-4xl font-bold mb-2 text-center relative">
          Radio En Vivo
          <span class="absolute -bottom-1 left-0 right-0 h-0.5 bg-tertiary/80 transform translate-y-1 rounded-full"></span>
        </h1>
        <p class="text-white text-lg md:text-xl text-center max-w-2xl">
          Escucha nuestra programación en vivo
        </p>
      </div>
    </div>
    
    <div class="p-6">
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-start">
        <!-- Reproductor y Descripción -->
        <div class="order-2 lg:order-1 flex flex-col items-center">
          <div class="mb-8 w-full max-w-md mx-auto">
            <RadioPlayer 
              showMiniPlayer={false} 
              stationName="CDITI Radio" 
              streamUrl={RADIO_URL} 
            />
          </div>
          
          <div class="bg-white rounded-xl shadow-sm border border-tertiary/10 p-6 w-full max-w-md">
            <h2 class="text-xl font-semibold text-secondary mb-4 relative inline-block">
              Sobre nuestra radio
              <span class="absolute -bottom-1 left-0 right-0 h-0.5 bg-tertiary"></span>
            </h2>
            <p class="text-secondary/80 text-sm mb-4">
              CDITI Radio es la emisora oficial del Centro de Diseño e Innovación Tecnológica Industrial del SENA Regional Risaralda. Ofrecemos una programación variada con enfoque educativo, tecnológico y cultural.
            </p>
            <p class="text-secondary/80 text-sm">
              Nuestra misión es mantener informada a la comunidad SENA y al público general sobre las actividades, proyectos y logros de nuestros aprendices e instructores, además de ofrecer contenido educativo de calidad.
            </p>
          </div>
        </div>
        
        <!-- Programación -->
        <div class="order-1 lg:order-2">
          <div class="bg-white rounded-xl shadow-sm border border-tertiary/10 p-6">
            <h2 class="text-xl font-semibold text-secondary mb-6 flex items-center relative inline-block">
              Programación Semanal
              <span class="absolute -bottom-1 left-0 right-0 h-0.5 bg-tertiary"></span>
            </h2>
            
            <div class="space-y-5 mt-4">
              <!-- Lunes a Viernes -->
              <div>
                <h3 class="text-primary font-medium mb-3 flex items-center">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
                  </svg>
                  Lunes a Viernes
                </h3>
                <ul class="space-y-4">
                  <li class="flex p-3 bg-secondary/5 rounded-lg">
                    <span class="text-secondary/80 w-24 text-sm font-medium">7:00 - 9:00</span>
                    <div>
                      <span class="text-secondary font-medium text-sm">Despierta CDITI</span>
                      <p class="text-secondary/70 text-xs">Actualidad, música y noticias para comenzar el día</p>
                    </div>
                  </li>
                  <li class="flex p-3 bg-tertiary/5 rounded-lg">
                    <span class="text-secondary/80 w-24 text-sm font-medium">10:00 - 12:00</span>
                    <div>
                      <span class="text-secondary font-medium text-sm">Innovación Tecnológica</span>
                      <p class="text-secondary/70 text-xs">Últimas tendencias en tecnología y innovación</p>
                    </div>
                  </li>
                  <li class="flex p-3 bg-primary/5 rounded-lg">
                    <span class="text-secondary/80 w-24 text-sm font-medium">14:00 - 16:00</span>
                    <div>
                      <span class="text-secondary font-medium text-sm">Entrevistas SENA</span>
                      <p class="text-secondary/70 text-xs">Conversaciones con instructores, aprendices y expertos</p>
                    </div>
                  </li>
                </ul>
              </div>
              
              <!-- Sábados -->
              <div>
                <h3 class="text-primary font-medium mb-3 flex items-center">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
                  </svg>
                  Sábados
                </h3>
                <ul class="space-y-4">
                  <li class="flex p-3 bg-alternative/5 rounded-lg">
                    <span class="text-secondary/80 w-24 text-sm font-medium">9:00 - 11:00</span>
                    <div>
                      <span class="text-secondary font-medium text-sm">Proyectos Destacados</span>
                      <p class="text-secondary/70 text-xs">Presentación de los mejores proyectos de nuestros aprendices</p>
                    </div>
                  </li>
                  <li class="flex p-3 bg-tertiary/5 rounded-lg">
                    <span class="text-secondary/80 w-24 text-sm font-medium">13:00 - 15:00</span>
                    <div>
                      <span class="text-secondary font-medium text-sm">Cultura y Región</span>
                      <p class="text-secondary/70 text-xs">Lo mejor de la cultura en Risaralda</p>
                    </div>
                  </li>
                </ul>
              </div>
            </div>
            
            <div class="mt-6 pt-6 border-t border-gray-100">
              <a href="/contacto" class="inline-flex items-center text-sm font-medium text-primary hover:text-tertiary transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
                ¿Preguntas sobre nuestra programación? Contáctanos
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  
  <!-- Llamada a la acción -->
  <div class="mb-12 bg-white rounded-xl shadow-md overflow-hidden">
    <div class="bg-gradient-to-br from-primary/20 to-primary/5 p-8 text-center">
      <h2 class="text-2xl font-bold text-secondary mb-4 relative inline-block">
        ¿Tienes un programa que quieres compartir?
        <span class="absolute -bottom-1 left-0 right-0 h-0.5 bg-tertiary/80"></span>
      </h2>
      <p class="text-secondary/80 mb-6 max-w-xl mx-auto">
        Si eres instructor o aprendiz del SENA y tienes una idea para un programa de radio, nos encantaría escucharte.
      </p>
      <a 
        href="/contacto" 
        class="inline-flex items-center px-5 py-2.5 bg-primary hover:bg-primary/90 text-white font-medium rounded-lg transition-all hover:scale-105"
      >
        Propón tu programa
        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 ml-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
        </svg>
      </a>
    </div>
  </div>
</main> 