<script lang="ts">
  interface BlogPost {
    title: string;
    excerpt: string;
    date: string;
    slug: string;
    coverImage?: string;
  }

  const { post } = $props<{ post: BlogPost }>();
  let isHovered = $state(false);

  const formattedDate = new Date(post.date).toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  });
</script>

<a 
  href={`/blog/${post.slug}`}
  class="group block bg-white dark:bg-gray-900 rounded-lg overflow-hidden shadow-lg transition-transform duration-300 hover:-translate-y-1"
  on:mouseenter={() => isHovered = true}
  on:mouseleave={() => isHovered = false}
>
  {#if post.coverImage}
    <div class="relative aspect-video overflow-hidden">
      <img 
        src={post.coverImage} 
        alt={post.title}
        class="w-full h-full object-cover transition-transform duration-500 ease-out"
        class:scale-110={isHovered}
      />
    </div>
  {/if}

  <div class="p-6">
    <p class="text-sm text-gray-500 dark:text-gray-400 mb-2">
      {formattedDate}
    </p>
    
    <h2 class="text-2xl font-semibold text-gray-900 dark:text-white mb-3 group-hover:text-blue-600 dark:group-hover:text-blue-400 transition-colors">
      {post.title}
    </h2>
    
    <p class="text-gray-600 dark:text-gray-300">
      {post.excerpt}
    </p>
    
    <div class="mt-4 flex items-center text-blue-600 dark:text-blue-400">
      <span class="text-sm font-medium">Read more</span>
      <svg class="w-4 h-4 ml-1 transition-transform group-hover:translate-x-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
      </svg>
    </div>
  </div>
</a>
