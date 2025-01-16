<script lang="ts">
  import { onMount } from 'svelte';
  import { fade } from 'svelte/transition';
  import AlbumCard from '$lib/components/AlbumCard.svelte';
  import Navigation from '$lib/components/Navigation.svelte';

  interface Album {
    title: string;
    coverImage: string;
    isNsfw: boolean;
    slug: string;
  }

  let albums = $state<Album[]>([]);
  let isLoading = $state(true);

  onMount(async () => {
    try {
      // In a real app, this would be an API call to fetch albums
      // For now, we'll simulate loading time
      await new Promise(resolve => setTimeout(resolve, 1000));
      
      // Example data
      albums = [
        {
          title: 'Nature',
          coverImage: '/images/albums/nature/cover.jpg',
          isNsfw: false,
          slug: 'nature'
        },
        // Add more albums here
      ];
    } finally {
      isLoading = false;
    }
  });
</script>

<Navigation />

<main class="min-h-screen bg-gray-900 px-4 py-16">
  <div class="max-w-7xl mx-auto">
    <h1 class="text-4xl md:text-5xl font-light text-white mb-12 text-center">
      Gallery
    </h1>

    {#if isLoading}
      <div class="flex justify-center items-center h-64">
        <div class="animate-spin rounded-full h-12 w-12 border-t-2 border-b-2 border-white"></div>
      </div>
    {:else}
      <div 
        class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8"
        in:fade={{ duration: 300 }}
      >
        {#each albums as album (album.slug)}
          <div class="transform hover:scale-105 transition-transform duration-300">
            <AlbumCard {album} />
          </div>
        {/each}
      </div>
    {/if}
  </div>
</main> 