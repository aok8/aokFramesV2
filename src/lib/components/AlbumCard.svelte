<script lang="ts">
  interface Album {
    title: string;
    coverImage: string;
    isNsfw: boolean;
    slug: string;
  }

  const { album } = $props<{ album: Album }>();
  let isHovered = $state(false);
</script>

<a 
  href={`/gallery/${album.slug}`}
  class="relative block aspect-square overflow-hidden group"
  on:mouseenter={() => isHovered = true}
  on:mouseleave={() => isHovered = false}
>
  <img 
    src={album.coverImage} 
    alt={album.title}
    class="w-full h-full object-cover transition-transform duration-500 ease-out"
    class:scale-110={isHovered}
    class:blur-lg={album.isNsfw}
  />
  
  <div 
    class="absolute inset-0 bg-black bg-opacity-0 transition-opacity duration-300"
    class:bg-opacity-50={isHovered}
  >
    {#if album.isNsfw}
      <span class="absolute top-4 right-4 bg-red-500 text-white px-2 py-1 text-sm rounded">
        NSFW
      </span>
    {/if}
    
    <div 
      class="absolute inset-0 flex items-center justify-center opacity-0 transition-opacity duration-300"
      class:opacity-100={isHovered}
    >
      <h2 class="text-white text-2xl font-light tracking-wider">
        {album.title}
      </h2>
    </div>
  </div>
</a>
