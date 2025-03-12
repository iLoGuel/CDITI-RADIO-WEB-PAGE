<script>
// @ts-nocheck
</script>

<svelte:head>
  <title>Contacto | CDITI Radio</title>
  <meta name="description" content="Contacta con el Centro de Diseño e Innovación Tecnológica Industrial SENA - Radio CDITI" />
</svelte:head>

<main class="max-w-7xl px-4 py-5 mx-auto">
  <div class="w-full bg-white rounded-xl overflow-hidden shadow-md mb-12">
    <!-- Banner con gradiente de la paleta de colores -->
    <div 
      class="w-full h-48 bg-gradient-to-r from-secondary via-secondary/90 to-secondary/80 relative"
    >
      <!-- Patrón de fondo decorativo -->
      <div class="absolute inset-0 opacity-10">
        <div class="absolute inset-0" style="background-image: radial-gradient(circle at 25px 25px, rgba(255, 255, 255, 0.2) 2%, transparent 0%), radial-gradient(circle at 75px 75px, rgba(255, 255, 255, 0.2) 2%, transparent 0%); background-size: 100px 100px;"></div>
      </div>
      
      <div class="w-full h-full flex flex-col justify-center items-center p-6 relative z-10">
        <!-- Elemento decorativo con gradiente -->
        <div class="absolute top-0 left-0 right-0 h-1 bg-gradient-to-r from-primary via-tertiary to-alternative"></div>
        
        <h1 class="text-white text-3xl md:text-4xl font-bold mb-2 text-center relative">
          Contacto
          <span class="absolute -bottom-1 left-0 right-0 h-0.5 bg-tertiary/80 transform translate-y-1 rounded-full"></span>
        </h1>
        <p class="text-white text-lg md:text-xl text-center max-w-2xl">
          Estamos aquí para escucharte
        </p>
      </div>
    </div>
    
    <div class="p-6">
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
        <!-- Mapa -->
        <div class="bg-secondary/5 rounded-xl overflow-hidden shadow-sm border border-secondary/10">
          <iframe
            class="w-full h-[450px]"
            src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d993.9031662581189!2d-75.6812239588102!3d4.8363825843164925!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x8e3881a4b50bb31d%3A0x9150c2e299ed35b0!2sCDITI%20SENA%20Dosquebradas!5e0!3m2!1ses-419!2sco!4v1715701456404!5m2!1ses-419!2sco"
            style="border:0;"
            allowfullscreen=""
            loading="lazy"
            referrerpolicy="no-referrer-when-downgrade"
            title="Ubicación del CDITI SENA Dosquebradas"
          ></iframe>
        </div>
        
        <!-- Información de contacto -->
        <div class="flex flex-col">
          <div class="bg-white rounded-xl shadow-sm border border-tertiary/10 p-6 mb-6">
            <h2 class="text-xl font-semibold text-secondary mb-4 relative inline-block">
              Ubicación
              <span class="absolute -bottom-1 left-0 right-0 h-0.5 bg-tertiary"></span>
            </h2>
            
            <div class="flex items-start mt-4">
              <div class="flex-shrink-0 mr-4">
                <div class="bg-primary/10 rounded-full p-3">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" />
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
                  </svg>
                </div>
              </div>
              <div>
                <h3 class="text-secondary font-medium">SENA - CENTRO DE DISEÑO E INNOVACIÓN TECNOLÓGICA INDUSTRIAL</h3>
                <p class="text-secondary/70 mt-1">Transv. 7 Calle 26 Barrio Santa Isabel</p>
                <p class="text-secondary/70">Dosquebradas, Risaralda, Colombia</p>
              </div>
            </div>
          </div>
          
          <div class="bg-white rounded-xl shadow-sm border border-tertiary/10 p-6">
            <h2 class="text-xl font-semibold text-secondary mb-4 relative inline-block">
              Información de Contacto
              <span class="absolute -bottom-1 left-0 right-0 h-0.5 bg-tertiary"></span>
            </h2>
            
            <div class="space-y-6 mt-4">
              <div class="flex items-start">
                <div class="flex-shrink-0 mr-4">
                  <div class="bg-primary/10 rounded-full p-3">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
                    </svg>
                  </div>
                </div>
                <div>
                  <h3 class="text-secondary font-medium">Correo Electrónico</h3>
                  <a href="mailto:cditiradio@sena.edu.co" class="text-primary hover:text-tertiary transition-colors">cditiradio@sena.edu.co</a>
                </div>
              </div>
              
              <div class="flex items-start">
                <div class="flex-shrink-0 mr-4">
                  <div class="bg-primary/10 rounded-full p-3">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z" />
                    </svg>
                  </div>
                </div>
                <div>
                  <h3 class="text-secondary font-medium">Teléfono</h3>
                  <p class="text-secondary/70">(+57) 606 313 5800</p>
                </div>
              </div>
              
              <div class="flex items-start">
                <div class="flex-shrink-0 mr-4">
                  <div class="bg-primary/10 rounded-full p-3">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                    </svg>
                  </div>
                </div>
                <div>
                  <h3 class="text-secondary font-medium">Horario de Atención</h3>
                  <p class="text-secondary/70">Lunes a Viernes: 8:00 AM - 5:00 PM</p>
                  <p class="text-secondary/70">Sábados: 8:00 AM - 12:00 PM</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  
  <!-- Sección de redes sociales -->
  <div class="w-full bg-white rounded-xl shadow-md p-6 mb-12">
    <h2 class="text-xl font-semibold text-secondary mb-6 relative inline-block">
      Síguenos en redes sociales
      <span class="absolute -bottom-1 left-0 right-0 h-0.5 bg-tertiary"></span>
    </h2>
    
    <div class="flex gap-6 justify-center md:justify-start flex-wrap">
      <a href="#" class="text-2xl hover:text-primary transition-colors flex flex-col items-center">
        <svg class="h-7 w-7" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
          <path fill-rule="evenodd" d="M22 12c0-5.523-4.477-10-10-10S2 6.477 2 12c0 4.991 3.657 9.128 8.438 9.878v-6.987h-2.54V12h2.54V9.797c0-2.506 1.492-3.89 3.777-3.89 1.094 0 2.238.195 2.238.195v2.46h-1.26c-1.243 0-1.63.771-1.63 1.562V12h2.773l-.443 2.89h-2.33v6.988C18.343 21.128 22 16.991 22 12z" clip-rule="evenodd" />
        </svg>
        <span class="text-xs mt-1 text-secondary/80">Facebook</span>
      </a>
      <a href="#" class="text-2xl hover:text-tertiary transition-colors flex flex-col items-center">
        <svg class="h-7 w-7" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
          <path fill-rule="evenodd" d="M12.315 2c2.43 0 2.784.013 3.808.06 1.064.049 1.791.218 2.427.465a4.902 4.902 0 011.772 1.153 4.902 4.902 0 011.153 1.772c.247.636.416 1.363.465 2.427.048 1.067.06 1.407.06 4.123v.08c0 2.643-.012 2.987-.06 4.043-.049 1.064-.218 1.791-.465 2.427a4.902 4.902 0 01-1.153 1.772 4.902 4.902 0 01-1.772 1.153c-.636.247-1.363.416-2.427.465-1.067.048-1.407.06-4.123.06h-.08c-2.643 0-2.987-.012-4.043-.06-1.064-.049-1.791-.218-2.427-.465a4.902 4.902 0 01-1.772-1.153 4.902 4.902 0 01-1.153-1.772c-.247-.636-.416-1.363-.465-2.427-.047-1.024-.06-1.379-.06-3.808v-.63c0-2.43.013-2.784.06-3.808.049-1.064.218-1.791.465-2.427a4.902 4.902 0 011.153-1.772A4.902 4.902 0 015.45 2.525c.636-.247 1.363-.416 2.427-.465C8.901 2.013 9.256 2 11.685 2h.63zm-.081 1.802h-.468c-2.456 0-2.784.011-3.807.058-.975.045-1.504.207-1.857.344-.467.182-.8.398-1.15.748-.35.35-.566.683-.748 1.15-.137.353-.3.882-.344 1.857-.047 1.023-.058 1.351-.058 3.807v.468c0 2.456.011 2.784.058 3.807.045.975.207 1.504.344 1.857.182.466.399.8.748 1.15.35.35.683.566 1.15.748.353.137.882.3 1.857.344 1.054.048 1.37.058 4.041.058h.08c2.597 0 2.917-.01 3.96-.058.976-.045 1.505-.207 1.858-.344.466-.182.8-.398 1.15-.748.35-.35.566-.683.748-1.15.137-.353.3-.882.344-1.857.048-1.055.058-1.37.058-4.041v-.08c0-2.597-.01-2.917-.058-3.96-.045-.976-.207-1.505-.344-1.858a3.097 3.097 0 00-.748-1.15 3.098 3.098 0 00-1.15-.748c-.353-.137-.882-.3-1.857-.344-1.023-.047-1.351-.058-3.807-.058zM12 6.865a5.135 5.135 0 110 10.27 5.135 5.135 0 010-10.27zm0 1.802a3.333 3.333 0 100 6.666 3.333 3.333 0 000-6.666zm5.338-3.205a1.2 1.2 0 110 2.4 1.2 1.2 0 010-2.4z" clip-rule="evenodd" />
        </svg>
        <span class="text-xs mt-1 text-secondary/80">Instagram</span>
      </a>
      <a href="#" class="text-2xl hover:text-alternative transition-colors flex flex-col items-center">
        <svg class="h-7 w-7" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
          <path d="M8.29 20.251c7.547 0 11.675-6.253 11.675-11.675 0-.178 0-.355-.012-.53A8.348 8.348 0 0022 5.92a8.19 8.19 0 01-2.357.646 4.118 4.118 0 001.804-2.27 8.224 8.224 0 01-2.605.996 4.107 4.107 0 00-6.993 3.743 11.65 11.65 0 01-8.457-4.287 4.106 4.106 0 001.27 5.477A4.072 4.072 0 012.8 9.713v.052a4.105 4.105 0 003.292 4.022 4.095 4.095 0 01-1.853.07 4.108 4.108 0 003.834 2.85A8.233 8.233 0 012 18.407a11.616 11.616 0 006.29 1.84" />
        </svg>
        <span class="text-xs mt-1 text-secondary/80">Twitter</span>
      </a>
    </div>
  </div>
</main>
