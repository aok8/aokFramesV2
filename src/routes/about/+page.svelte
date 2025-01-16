<script lang="ts">
  import { onMount } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  import Navigation from '$lib/components/Navigation.svelte';

  let isVisible = $state(false);

  onMount(() => {
    setTimeout(() => {
      isVisible = true;
    }, 300);
  });
</script>

<Navigation />

<div class="relative min-h-screen">
  <!-- Background image with overlay -->
  <div 
    class="absolute inset-0 bg-cover bg-center bg-no-repeat"
    style="background-image: url('/images/about-bg.jpg');"
  >
    <div class="absolute inset-0 bg-black bg-opacity-60"></div>
  </div>

  <!-- Content -->
  <main class="relative min-h-screen flex items-center justify-center px-4 py-16">
    <div class="max-w-3xl">
      {#if isVisible}
        <div 
          class="space-y-6 text-white"
          in:fly={{ y: 50, duration: 1000 }}
          out:fade
        >
          <h1 class="text-4xl md:text-5xl font-light mb-8 text-center">
            About Me
          </h1>
          
          <p class="text-lg font-light leading-relaxed">
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed vitae justo nec nunc facilisis tincidunt. 
            In hac habitasse platea dictumst. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices 
            posuere cubilia curae.
          </p>

          <p class="text-lg font-light leading-relaxed">
            Praesent euismod, nisi vel consectetur euismod, nunc nisi tincidunt nunc, vitae aliquam nunc nisi 
            vitae nunc. Sed vitae justo nec nunc facilisis tincidunt. In hac habitasse platea dictumst.
          </p>

          <p class="text-lg font-light leading-relaxed">
            Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae. Praesent 
            euismod, nisi vel consectetur euismod, nunc nisi tincidunt nunc, vitae aliquam nunc nisi vitae nunc.
          </p>
        </div>
      {/if}
    </div>
  </main>
</div> 