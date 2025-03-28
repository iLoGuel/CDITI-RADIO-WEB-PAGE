<script>
  import { inview } from 'svelte-inview';
  
  // Propiedades configurables
  export let animation = 'fade'; // fade, slide-up, slide-down, slide-left, slide-right, zoom
  export let delay = 0; // milliseconds
  export let duration = 600; // milliseconds
  export let threshold = 0.2; // porcentaje del elemento visible para activar
  export let once = true; // activar solo una vez

  // Estado interno
  let isInView = false;
  let node;
  
  // Opciones para inview
  const options = {
    root: null,
    rootMargin: '0px',
    threshold,
    unobserveOnEnter: once
  };

  // Función que se ejecuta cuando el elemento entra o sale del viewport
  function handleChange({ detail }) {
    if (detail.inView) {
      isInView = true;
    } else if (!once) {
      isInView = false;
    }
  }
  
  // Mapeo de animaciones a clases CSS
  const animationClasses = {
    'fade': 'opacity-0 transition-opacity',
    'slide-up': 'translate-y-16 opacity-0 transition-transform transition-opacity',
    'slide-down': '-translate-y-16 opacity-0 transition-transform transition-opacity',
    'slide-left': 'translate-x-16 opacity-0 transition-transform transition-opacity',
    'slide-right': '-translate-x-16 opacity-0 transition-transform transition-opacity',
    'zoom': 'scale-90 opacity-0 transition-transform transition-opacity',
  };
</script>

<div
  bind:this={node}
  use:inview={options}
  on:inview_change={handleChange}
  class={`${animationClasses[animation]} ${isInView ? 'opacity-100 translate-x-0 translate-y-0 scale-100' : ''}`}
  style={`transition-duration: ${duration}ms; transition-delay: ${delay}ms;`}
>
  <slot />
</div>

<style>
  div {
    transition-property: opacity, transform;
    transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  }
</style> 