<script lang="ts">
  import { onMount } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  import Navigation from '$lib/components/Navigation.svelte';

  let scrollY: number = 0;
  let innerHeight: number = 0;
  let coverTransform = $state(0);

  // Update transform based on scroll position
  $effect(() => {
    if (typeof scrollY === 'undefined' || typeof innerHeight === 'undefined') return;
    const progress = Math.min(scrollY / innerHeight, 1);
    coverTransform = progress * 100;
  });
</script>

<svelte:window bind:scrollY bind:innerHeight />

<!-- Navigation should be outside both layers to always be accessible -->
<Navigation />

<!-- Cover page -->
<div 
  class="fixed inset-0 bg-[#2D4739] z-20 flex items-center justify-center pointer-events-none"
  style="transform: translateX(-{coverTransform}%); transition: transform 0.5s ease-out"
>
  <h1 class="text-6xl md:text-8xl text-white font-light tracking-wider">
    AOKFrames
  </h1>
</div>

<!-- Scrollable container for triggering the animation -->
<div class="h-[200vh]">
  <!-- Fixed content container -->
  <div class="fixed inset-0 overflow-hidden">
    <!-- Background with slide effect -->
    <div 
      class="absolute inset-0 bg-cover bg-center bg-no-repeat"
      style="transform: translateX(-{coverTransform}%); transition: transform 0.5s ease-out; background-image: url('/images/bg.jpg');"
    >
      <div class="absolute inset-0 bg-black bg-opacity-40"></div>
    </div>

    <!-- Main content - always centered -->
    <main class="relative flex items-center justify-center h-full text-white">
      <h1 
        class="text-5xl md:text-7xl font-light tracking-wider"
        style="opacity: {coverTransform / 100}; transition: opacity 0.5s ease-out"
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
