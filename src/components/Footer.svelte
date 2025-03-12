<script>
  import logoSennova from "$lib/images/sennova-logo.png";
  import logoSena from "$lib/images/sena-logo.svg";
</script>

<footer class="bg-secondary/5 border-t border-tertiary/10">
  <!-- Banda superior de color -->
  <div class="h-1 bg-gradient-to-r from-alternative via-tertiary to-primary"></div>
  
  <!-- Sección principal de contenido -->
  <div class="max-w-7xl mx-auto px-5 py-12">
    <!-- Logos e información institucional centrados -->
    <div class="flex flex-col items-center justify-center mb-10 pb-8 border-b border-tertiary/20">
      <div class="flex items-center gap-6 mb-5">
        <img class="h-14" src={logoSena} alt="Logo de SENA" />
        <div class="h-14 w-px bg-primary/40"></div>
        <img class="h-12" src={logoSennova} alt="Logo de SENNOVA" />
      </div>
      <p class="text-sm text-secondary/90 text-center max-w-md">
        Centro de Diseño e Innovación Tecnológica Industrial<br>
        <span class="text-primary font-medium">Regional Risaralda</span>
      </p>
    </div>
    
    <!-- Información de contacto en 3 columnas uniformes -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
      <!-- Columna 1: Ubicación -->
      <div class="flex flex-col items-center text-center">
        <h3 class="text-secondary font-bold mb-4 text-sm relative inline-block">
          <span class="after:content-[''] after:absolute after:w-full after:h-0.5 after:bg-tertiary after:bottom-0 after:left-0">
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
          <p class="text-xs text-secondary/80 max-w-xs">
            Transv. 7 Calle 26 Barrio Santa Isabel, Dosquebradas, Risaralda
          </p>
        </div>
      </div>
      
      <!-- Columna 2: Contacto -->
      <div class="flex flex-col items-center text-center">
        <h3 class="text-secondary font-bold mb-4 text-sm relative inline-block">
          <span class="after:content-[''] after:absolute after:w-full after:h-0.5 after:bg-alternative after:bottom-0 after:left-0">
            CONTACTO
          </span>
        </h3>
        <div class="mt-5 flex flex-col items-center">
          <span class="bg-alternative/10 p-2.5 rounded-full mb-3">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-alternative" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
            </svg>
          </span>
          <a 
            href="mailto:cditi@sena.edu.co" 
            class="text-xs text-secondary/80 hover:text-primary transition-colors"
          >
            cditi@sena.edu.co
          </a>
        </div>
      </div>
      
      <!-- Columna 3: Redes Sociales -->
      <div class="flex flex-col items-center text-center">
        <h3 class="text-secondary font-bold mb-4 text-sm relative inline-block">
          <span class="after:content-[''] after:absolute after:w-full after:h-0.5 after:bg-primary after:bottom-0 after:left-0">
            SÍGUENOS
          </span>
        </h3>
        <div class="mt-5 flex gap-4">
          <a 
            href="https://facebook.com" 
            target="_blank" 
            rel="noopener noreferrer" 
            class="text-secondary/70 hover:text-primary transition-colors"
          >
            <span class="sr-only">Facebook</span>
            <svg class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
              <path fill-rule="evenodd" d="M22 12c0-5.523-4.477-10-10-10S2 6.477 2 12c0 4.991 3.657 9.128 8.438 9.878v-6.987h-2.54V12h2.54V9.797c0-2.506 1.492-3.89 3.777-3.89 1.094 0 2.238.195 2.238.195v2.46h-1.26c-1.243 0-1.63.771-1.63 1.562V12h2.773l-.443 2.89h-2.33v6.988C18.343 21.128 22 16.991 22 12z" clip-rule="evenodd" />
            </svg>
          </a>
          <a 
            href="https://instagram.com" 
            target="_blank" 
            rel="noopener noreferrer" 
            class="text-secondary/70 hover:text-tertiary transition-colors"
          >
            <span class="sr-only">Instagram</span>
            <svg class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
              <path fill-rule="evenodd" d="M12.315 2c2.43 0 2.784.013 3.808.06 1.064.049 1.791.218 2.427.465a4.902 4.902 0 011.772 1.153 4.902 4.902 0 011.153 1.772c.247.636.416 1.363.465 2.427.048 1.067.06 1.407.06 4.123v.08c0 2.643-.012 2.987-.06 4.043-.049 1.064-.218 1.791-.465 2.427a4.902 4.902 0 01-1.153 1.772 4.902 4.902 0 01-1.772 1.153c-.636.247-1.363.416-2.427.465-1.067.048-1.407.06-4.123.06h-.08c-2.643 0-2.987-.012-4.043-.06-1.064-.049-1.791-.218-2.427-.465a4.902 4.902 0 01-1.772-1.153 4.902 4.902 0 01-1.153-1.772c-.247-.636-.416-1.363-.465-2.427-.047-1.024-.06-1.379-.06-3.808v-.63c0-2.43.013-2.784.06-3.808.049-1.064.218-1.791.465-2.427a4.902 4.902 0 011.153-1.772A4.902 4.902 0 015.45 2.525c.636-.247 1.363-.416 2.427-.465C8.901 2.013 9.256 2 11.685 2h.63zm-.081 1.802h-.468c-2.456 0-2.784.011-3.807.058-.975.045-1.504.207-1.857.344-.467.182-.8.398-1.15.748-.35.35-.566.683-.748 1.15-.137.353-.3.882-.344 1.857-.047 1.023-.058 1.351-.058 3.807v.468c0 2.456.011 2.784.058 3.807.045.975.207 1.504.344 1.857.182.466.399.8.748 1.15.35.35.683.566 1.15.748.353.137.882.3 1.857.344 1.054.048 1.37.058 4.041.058h.08c2.597 0 2.917-.01 3.96-.058.976-.045 1.505-.207 1.858-.344.466-.182.8-.398 1.15-.748.35-.35.566-.683.748-1.15.137-.353.3-.882.344-1.857.048-1.055.058-1.37.058-4.041v-.08c0-2.597-.01-2.917-.058-3.96-.045-.976-.207-1.505-.344-1.858a3.097 3.097 0 00-.748-1.15 3.098 3.098 0 00-1.15-.748c-.353-.137-.882-.3-1.857-.344-1.023-.047-1.351-.058-3.807-.058zM12 6.865a5.135 5.135 0 110 10.27 5.135 5.135 0 010-10.27zm0 1.802a3.333 3.333 0 100 6.666 3.333 3.333 0 000-6.666zm5.338-3.205a1.2 1.2 0 110 2.4 1.2 1.2 0 010-2.4z" clip-rule="evenodd" />
            </svg>
          </a>
          <a 
            href="https://twitter.com"
            target="_blank" 
            rel="noopener noreferrer"
            class="text-secondary/70 hover:text-alternative transition-colors"
          >
            <span class="sr-only">Twitter</span>
            <svg class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
              <path d="M8.29 20.251c7.547 0 11.675-6.253 11.675-11.675 0-.178 0-.355-.012-.53A8.348 8.348 0 0022 5.92a8.19 8.19 0 01-2.357.646 4.118 4.118 0 001.804-2.27 8.224 8.224 0 01-2.605.996 4.107 4.107 0 00-6.993 3.743 11.65 11.65 0 01-8.457-4.287 4.106 4.106 0 001.27 5.477A4.072 4.072 0 012.8 9.713v.052a4.105 4.105 0 003.292 4.022 4.095 4.095 0 01-1.853.07 4.108 4.108 0 003.834 2.85A8.233 8.233 0 012 18.407a11.616 11.616 0 006.29 1.84" />
            </svg>
          </a>
        </div>
      </div>
    </div>
  </div>
  
  <!-- Derechos de autor -->
  <div class="bg-secondary text-white py-4">
    <div class="max-w-7xl mx-auto px-5 text-center text-xs">
      <p>© {new Date().getFullYear()} CDITI RADIO - Centro de Diseño e Innovación Tecnológica Industrial | SENA Regional Risaralda</p>
      <p class="mt-1 text-tertiary/90">Todos los derechos reservados</p>
    </div>
  </div>
</footer>