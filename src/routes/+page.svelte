<script lang="ts">
  import { onMount } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  import Navigation from '$lib/components/Navigation.svelte';

  let scrollY = $state(0);
  let innerHeight = $state(0);
  let coverTransform = $state(0);
  let mainTextOpacity = $state(0);
  let overlayOpacity = $state(0.4);
  let textAnimationStarted = $state(false);
  let textAnimationComplete = $state(false);
  let aboutTextOpacity = $state(0);
  let coverWasFullyRestored = $state(true); // Track if cover was fully back

  // Update transform based on scroll position
  $effect(() => {
    if (scrollY === undefined || innerHeight === undefined) return;
    
    // Calculate cover slide progress (0-100%)
    const slideProgress = Math.min(scrollY / innerHeight, 1);
    coverTransform = slideProgress * 100;

    // Check if cover is fully restored (at the very top)
    if (scrollY <= 10) { // Small threshold for better UX
      coverWasFullyRestored = true;
    }

    // Reset text animation if cover comes back
    if (slideProgress < 1) {
      textAnimationStarted = false;
      textAnimationComplete = false;
      mainTextOpacity = 0;
      aboutTextOpacity = 0;
    }

    // Start text animation when cover is fully removed AND cover was fully restored before
    if (slideProgress >= 1 && !textAnimationStarted && coverWasFullyRestored) {
      coverWasFullyRestored = false; // Reset the flag
      textAnimationStarted = true;
      document.body.style.overflow = 'hidden'; // Lock scrolling
      // Fade in text
      setTimeout(() => {
        mainTextOpacity = 1;
        // Fade out text after a delay
        setTimeout(() => {
          mainTextOpacity = 0;
          setTimeout(() => {
            textAnimationComplete = true;
            document.body.style.overflow = ''; // Unlock scrolling
            // Auto-scroll slightly to hint at more content
            window.scrollTo({
              top: window.innerHeight * 1.1,
              behavior: 'smooth'
            });
          }, 500);
        }, 1500);
      }, 200);
    }

    // Handle about text opacity based on scroll position after animation
    if (textAnimationComplete && scrollY > innerHeight * 2) {
      const aboutScrollProgress = (scrollY - innerHeight * 2) / (innerHeight * 0.5);
      aboutTextOpacity = Math.min(aboutScrollProgress, 1);
    }
  });

  // Reset overflow when component is destroyed
  onMount(() => {
    return () => {
      document.body.style.overflow = '';
    };
  });
</script>

<svelte:window bind:scrollY bind:innerHeight />

<Navigation />

<!-- Scrollable container -->
<div class="min-h-[300vh]">
  <!-- First section with background image -->
  <div class="fixed inset-0 overflow-hidden">
    <!-- Background image -->
    <div 
      class="absolute inset-0 bg-cover bg-center bg-no-repeat"
      style="background-image: url('/images/bg.jpg');"
    >
      <div 
        class="absolute inset-0 bg-black transition-opacity duration-500"
        style="opacity: {overlayOpacity}"
      ></div>
    </div>

    <!-- Sliding green overlay -->
    <div 
      class="absolute inset-0 z-20 pointer-events-none"
      style="transform: translateX(-{coverTransform}%); transition: transform 0.5s ease-out"
    >
      <div class="h-full w-full bg-[#2D4739]">
        <div class="h-full flex items-center justify-center">
          <h1 class="text-6xl md:text-8xl text-white font-light tracking-wider">
            AOKFrames
          </h1>
        </div>
      </div>
    </div>

    <!-- Main content - always centered -->
    <main class="relative flex items-center justify-center h-full text-white z-30">
      <h1 
        class="text-5xl md:text-7xl font-light tracking-wider transition-all duration-1000"
        style="opacity: {mainTextOpacity}"
      >
        growth through experience
      </h1>
    </main>
  </div>

  <!-- About section (scrolls into view) -->
  <div 
    class="absolute w-full min-h-screen bg-[#2D4739] flex items-center justify-center px-4 md:px-8"
    style="top: 210vh;"
  >
    <div 
      class="max-w-3xl text-white text-center"
      style="opacity: {aboutTextOpacity}; transition: opacity 1s ease-out;"
    >
      <h2 class="text-4xl md:text-5xl font-light mb-8">About</h2>
      <p class="text-xl md:text-2xl font-light leading-relaxed">
        I'm a Seattle based photographer shooting both in film and digital. Photography has been a main passion of mine and I enjoy sharing how I see the world. Being able to shoot in both mediums is great to me as I get to have the best of both worlds and also try different photographic experiences.
      </p>
    </div>
  </div>
</div>

<style>
  :global(body) {
    margin: 0;
    overflow-x: hidden;
    scroll-behavior: smooth;
  }

  :global(html) {
    scroll-behavior: smooth;
  }
</style>
