<script lang="ts">
  import { onMount } from 'svelte';
  import { fade } from 'svelte/transition';
  import BlogCard from '$lib/components/BlogCard.svelte';
  import Navigation from '$lib/components/Navigation.svelte';

  interface BlogPost {
    title: string;
    excerpt: string;
    date: string;
    slug: string;
    coverImage?: string;
  }

  let posts = $state<BlogPost[]>([]);
  let isLoading = $state(true);

  onMount(async () => {
    try {
      // simulate loading time
      await new Promise(resolve => setTimeout(resolve, 1000));
      
      // Example data
      posts = [
        {
          title: 'The Art of Photography',
          excerpt: 'Photography is not just about capturing moments; it\'s about telling stories through visual narratives...',
          date: '2024-01-15',
          slug: 'art-of-photography',
          coverImage: '/images/blog/photography-art.jpg'
        },
        // Add more posts here
      ];
    } finally {
      isLoading = false;
    }
  });
</script>

<Navigation />

<main class="min-h-screen bg-gray-100 dark:bg-gray-900 px-4 py-16">
  <div class="max-w-4xl mx-auto">
    <h1 class="text-4xl md:text-5xl font-light text-gray-900 dark:text-white mb-12 text-center">
      Blog
    </h1>

    {#if isLoading}
      <div class="flex justify-center items-center h-64">
        <div class="animate-spin rounded-full h-12 w-12 border-t-2 border-b-2 border-gray-900 dark:border-white"></div>
      </div>
    {:else}
      <div 
        class="space-y-8"
        in:fade={{ duration: 300 }}
      >
        {#each posts as post (post.slug)}
          <BlogCard {post} />
        {/each}
      </div>
    {/if}
  </div>
</main> 