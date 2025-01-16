<script lang="ts">
  import { onMount } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  import Navigation from '$lib/components/Navigation.svelte';

  let scrollY = $state(0);
  let innerHeight = $state(0);
  let coverTransform = $state(0);
  let mainTextOpacity = $state(0);
  let overlayOpacity = $state(0.4); // Initial overlay opacity

  // Update transform based on scroll position
  $effect(() => {
    if (scrollY === undefined || innerHeight === undefined) return;
    
    // First phase (0-100%): Slide out cover and fade in text
    const firstPhaseProgress = Math.min(scrollY / (innerHeight * 1.5), 1);
    coverTransform = firstPhaseProgress * 100;
    mainTextOpacity = firstPhaseProgress;

    // Second phase: Fade out text and overlay (starts after first phase)
    if (scrollY > innerHeight * 1.5) {
      const secondPhaseProgress = Math.min((scrollY - innerHeight * 1.5) / (innerHeight * 1.5), 1);
      mainTextOpacity = 1 - secondPhaseProgress;
      overlayOpacity = 0.4 * (1 - secondPhaseProgress);
    }
  });
</script>

<svelte:window bind:scrollY bind:innerHeight />

<!-- Navigation should be outside both layers to always be accessible -->
<Navigation />

<!-- Scrollable container for triggering the animation -->
<div class="h-[400vh]">
  <!-- Fixed content container -->
  <div class="fixed inset-0 overflow-hidden">
    <!-- Background image (stays in place) -->
    <div 
      class="absolute inset-0 bg-cover bg-center bg-no-repeat"
      style="background-image: url('/images/bg.jpg');"
    >
      <div 
        class="absolute inset-0 bg-black transition-opacity duration-500"
        style="opacity: {overlayOpacity}"
      ></div>
    </div>

    <!-- Sliding green background -->
    <div 
      class="absolute inset-0 bg-[#2D4739] z-20 pointer-events-none"
      style="transform: translateX(-{coverTransform}%); transition: transform 0.5s ease-out"
    >
      <div class="h-full flex items-center justify-center">
        <h1 class="text-6xl md:text-8xl text-white font-light tracking-wider">
          AOKFrames
        </h1>
      </div>
    </div>

    <!-- Main content - always centered -->
    <main class="relative flex items-center justify-center h-full text-white z-10">
      <h1 
        class="text-5xl md:text-7xl font-light tracking-wider transition-opacity duration-500"
        style="opacity: {mainTextOpacity}"
      >
        growth through experience
      </h1>
    </main>
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
