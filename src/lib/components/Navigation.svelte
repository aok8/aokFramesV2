<script lang="ts">
  import { slide } from 'svelte/transition';
  import { quintOut } from 'svelte/easing';

  let isMenuOpen = $state(false);

  const menuItems = [
    { href: '/', label: 'Home' },
    { href: '/gallery', label: 'Gallery' },
    { href: '/blog', label: 'Blog' },
    { href: 'https://aokframes.darkroom.com/', label: 'Prints', external: true },
    { href: '/about', label: 'About' },
    { href: '/contact', label: 'Contact' }
  ];

  function toggleMenu() {
    isMenuOpen = !isMenuOpen;
  }
</script>

<nav class="fixed top-0 right-0 z-40 p-4">
  <button 
    class="relative z-50 w-10 h-10 focus:outline-none"
    onclick={toggleMenu}
    aria-label="Toggle menu"
  >
    <div class="flex flex-col justify-center h-full space-y-2">
      <span 
        class="block w-8 h-0.5 bg-white transform transition-transform duration-300"
        class:rotate-45={isMenuOpen}
        class:translate-y-2.5={isMenuOpen}
      ></span>
      <span 
        class="block w-8 h-0.5 bg-white transition-opacity duration-300"
        class:opacity-0={isMenuOpen}
      ></span>
      <span 
        class="block w-8 h-0.5 bg-white transform transition-transform duration-300"
        class:-rotate-45={isMenuOpen}
        class:-translate-y-2.5={isMenuOpen}
      ></span>
    </div>
  </button>
</nav>

{#if isMenuOpen}
  <div 
    class="fixed inset-0 bg-black z-30"
    transition:slide={{ duration: 400, easing: quintOut }}
  >
    <div class="flex items-center justify-center h-full">
      <ul class="space-y-8 text-center">
        {#each menuItems as { href, label, external }}
          <li 
            class="transform hover:scale-110 transition-transform duration-200"
          >
            <a 
              {href} 
              class="text-white text-3xl font-light hover:text-gray-300"
              target={external ? '_blank' : undefined}
              rel={external ? 'noopener noreferrer' : undefined}
              onclick={() => !external && (isMenuOpen = false)}
            >
              {label}
            </a>
          </li>
        {/each}
      </ul>
    </div>
  </div>
{/if} 