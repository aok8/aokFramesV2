<script lang="ts">
  import { fade } from 'svelte/transition';

  interface Image {
    url: string;
    alt: string;
  }

  const { images } = $props<{ images: Image[] }>();
  
  let currentIndex = $state(0);
  let isTransitioning = $state(false);

  function next() {
    if (isTransitioning) return;
    isTransitioning = true;
    currentIndex = (currentIndex + 1) % images.length;
    setTimeout(() => isTransitioning = false, 300);
  }

  function previous() {
    if (isTransitioning) return;
    isTransitioning = true;
    currentIndex = (currentIndex - 1 + images.length) % images.length;
    setTimeout(() => isTransitioning = false, 300);
  }

  function handleKeydown(event: KeyboardEvent) {
    if (event.key === 'ArrowRight') next();
    if (event.key === 'ArrowLeft') previous();
  }
</script>

<svelte:window on:keydown={handleKeydown} />

<div class="relative w-full h-screen bg-black">
  <!-- Current image -->
  {#key currentIndex}
    <img 
      src={images[currentIndex].url}
      alt={images[currentIndex].alt}
      class="absolute inset-0 w-full h-full object-contain"
      in:fade={{ duration: 300 }}
    />
  {/key}

  <!-- Navigation buttons -->
  <button
    class="absolute left-4 top-1/2 -translate-y-1/2 text-white p-4 hover:bg-black hover:bg-opacity-25 rounded-full transition-colors"
    on:click={previous}
    aria-label="Previous image"
  >
    <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
    </svg>
  </button>

  <button
    class="absolute right-4 top-1/2 -translate-y-1/2 text-white p-4 hover:bg-black hover:bg-opacity-25 rounded-full transition-colors"
    on:click={next}
    aria-label="Next image"
  >
    <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
    </svg>
  </button>

  <!-- Image counter -->
  <div class="absolute bottom-4 left-1/2 -translate-x-1/2 text-white bg-black bg-opacity-50 px-4 py-2 rounded-full">
    {currentIndex + 1} / {images.length}
  </div>
</div>
