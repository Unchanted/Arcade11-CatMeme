<script>
  import { onMount } from 'svelte';
  
  export let className = "";
  export let containerClassName = "";
  export let colors = ["#0e0e0e", "#cf1e1e", "#ff2e2e"];

  let mounted = false;

  onMount(() => {
    mounted = true;
    loadExternalScripts().then(() => {
      initializeBackgroundGradient();
    });
  });

  function loadExternalScripts() {
    const scripts = [
      "https://unpkg.com/@studio-freight/lenis@1.0.27/dist/lenis.min.js",
      "https://unpkg.com/framer-motion@10.16.4/dist/framer-motion.min.js",
      "https://unpkg.com/react@18.2.0/umd/react.production.min.js",
      "https://unpkg.com/react-dom@18.2.0/umd/react-dom.production.min.js",
      "https://unpkg.com/@tsparticles/engine@3.0.2/tsparticles.engine.min.js",
      "https://unpkg.com/@tsparticles/slim@3.0.2/tsparticles.slim.min.js",
      "https://unpkg.com/aceternity-ui@1.0.0/dist/aceternity-ui.min.js"
    ];

    return Promise.all(scripts.map(src => {
      return new Promise((resolve, reject) => {
        const script = document.createElement('script');
        script.src = src;
        script.onload = resolve;
        script.onerror = reject;
        document.head.appendChild(script);
      });
    }));
  }

  function initializeBackgroundGradient() {
    if (window.aceternity && window.aceternity.BackgroundGradient) {
      const BackgroundGradient = window.aceternity.BackgroundGradient;
      const elements = document.querySelectorAll('.background-gradient-wrapper');
      
      elements.forEach(el => {
        const colors = JSON.parse(el.dataset.colors || '[]');
        const props = {
          containerClassName: 'background-gradient-container',
          className: 'background-gradient',
          colors: colors,
        };
        
        ReactDOM.render(React.createElement(BackgroundGradient, props, el.innerHTML), el);
      });
    }
  }
</script>

{#if mounted}
  <div class="background-gradient-wrapper" data-colors={JSON.stringify(colors)}>
    <div class={containerClassName}>
      <div class={className}>
        <slot />
      </div>
    </div>
  </div>
{/if}

<style>
  .background-gradient-wrapper {
    width: 100%;
    height: 100%;
    padding: 30px; /* Increase padding to make gradient more visible */
  }
</style>
