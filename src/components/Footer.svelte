<script>
  import logoSennova from "$lib/images/sennova-logo.png";
  import logoSena from "$lib/images/sena-logo.svg";
</script>

<footer class="bg-gray-50 border-t border-gray-100">
  <!-- Banda superior de color -->
  <div class="border-t-4 border-primary/80"></div>
  
  <!-- Sección principal de contenido -->
  <div class="max-w-7xl mx-auto px-5 py-12">
    <!-- Logos e información institucional centrados -->
    <div class="flex flex-col items-center justify-center mb-10 pb-8 border-b border-gray-200">
      <div class="flex items-center gap-6 mb-5">
        <img class="h-14" src={logoSena} alt="Logo de SENA" />
        <div class="h-14 w-px bg-primary/20"></div>
        <img class="h-12" src={logoSennova} alt="Logo de SENNOVA" />
      </div>
      <p class="text-sm text-gray-600 text-center max-w-md">
        Centro de Diseño e Innovación Tecnológica Industrial<br>
        <span class="text-primary font-medium">Regional Risaralda</span>
      </p>
    </div>
    
    <!-- Información de contacto en 3 columnas uniformes -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
      <!-- Columna 1: Ubicación -->
      <div class="flex flex-col items-center text-center">
        <h3 class="text-secondary font-bold mb-4 text-sm relative inline-block">
          <span class="after:content-[''] after:absolute after:w-full after:h-0.5 after:bg-tertiary/80 after:bottom-0 after:left-0">
            UBICACIÓN
          </span>
        </h3>
        <div class="mt-5 flex flex-col items-center">
          <span class="bg-primary/10 p-2.5 rounded-full mb-3">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" />
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
            </svg>
          </span>
          <p class="text-xs text-gray-600 max-w-xs">
            Transv. 7 Calle 26 Barrio Santa Isabel, Dosquebradas, Risaralda
          </p>
        </div>
      </div>
      
      <!-- Columna 2: Contacto -->
      <div class="flex flex-col items-center text-center">
        <h3 class="text-secondary font-bold mb-4 text-sm relative inline-block">
          <span class="after:content-[''] after:absolute after:w-full after:h-0.5 after:bg-tertiary/80 after:bottom-0 after:left-0">
            CONTACTO
          </span>
        </h3>
        <div class="mt-5 flex flex-col items-center space-y-5">
          <div class="flex flex-col items-center">
            <span class="bg-primary/10 p-2.5 rounded-full mb-3">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z" />
              </svg>
            </span>
            <p class="text-xs text-gray-600">
              +57 (6) 3113700 IP 63361
            </p>
          </div>
          <div class="flex flex-col items-center">
            <span class="bg-primary/10 p-2.5 rounded-full mb-3">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 12a4 4 0 10-8 0 4 4 0 008 0zm0 0v1.5a2.5 2.5 0 005 0V12a9 9 0 10-9 9m4.5-1.206a8.959 8.959 0 01-4.5 1.207" />
              </svg>
            </span>
            <p class="text-xs text-gray-600">
              contactenos@sena.edu.co
            </p>
          </div>
        </div>
      </div>
      
      <!-- Columna 3: Atención -->
      <div class="flex flex-col items-center text-center">
        <h3 class="text-secondary font-bold mb-4 text-sm relative inline-block">
          <span class="after:content-[''] after:absolute after:w-full after:h-0.5 after:bg-tertiary/80 after:bottom-0 after:left-0">
            ATENCIÓN AL CIUDADANO
          </span>
        </h3>
        <div class="mt-5 flex flex-col items-center space-y-5">
          <div class="flex flex-col items-center">
            <span class="bg-primary/10 p-2.5 rounded-full mb-3">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
            </span>
            <p class="text-xs text-gray-600">
              Lunes a viernes<br>
              8:00 a.m. a 6:00 p.m.
            </p>
          </div>
          <div class="flex flex-col items-center">
            <span class="bg-primary/10 p-2.5 rounded-full mb-3">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z" />
              </svg>
            </span>
            <p class="text-xs text-gray-600">
              Bogotá: (57 1) 3430111<br>
              Línea gratuita: 018000 910270<br>
              Conmutador: (57 1) 5461500
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
  
  <!-- Banda inferior con copyright -->
  <div class="bg-secondary/5 py-4 border-t border-gray-200">
    <div class="max-w-7xl mx-auto px-5 text-center">
      <p class="text-xs text-gray-500 mb-2">
        © {new Date().getFullYear()} Servicio Nacional de Aprendizaje SENA - Todos los derechos reservados
      </p>
      <div class="flex justify-center gap-6 text-xs">
        <a href="#" class="text-gray-500 hover:text-primary transition-colors">Política de privacidad</a>
        <a href="#" class="text-gray-500 hover:text-primary transition-colors">Términos de uso</a>
      </div>
    </div>
  </div>
</footer>