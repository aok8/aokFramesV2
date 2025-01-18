<script lang="ts">
  import { fade } from 'svelte/transition';
  import Navigation from '$lib/components/Navigation.svelte';

  let name = $state('');
  let email = $state('');
  let message = $state('');
  let isSubmitting = $state(false);
  let isSuccess = $state(false);
  let error = $state('');

  async function handleSubmit() {
    if (isSubmitting) return;

    isSubmitting = true;
    error = '';

    try {
      // In a real app, fake email send, still need to implement
      await new Promise(resolve => setTimeout(resolve, 1000));
      
      // Simulate success
      isSuccess = true;
      name = '';
      email = '';
      message = '';
    } catch (err) {
      error = 'Failed to send message. Please try again.';
    } finally {
      isSubmitting = false;
    }
  }
</script>

<Navigation />

<main 
  class="min-h-screen bg-gray-100 dark:bg-gray-900 px-4 py-16"
  in:fade={{ duration: 300 }}
>
  <div class="max-w-2xl mx-auto">
    <h1 class="text-4xl md:text-5xl font-light text-gray-900 dark:text-white mb-12 text-center">
      Contact
    </h1>

    {#if isSuccess}
      <div class="bg-green-100 dark:bg-green-900 p-6 rounded-lg text-center">
        <h2 class="text-2xl font-light text-green-900 dark:text-green-100 mb-2">
          Message Sent!
        </h2>
        <p class="text-green-800 dark:text-green-200">
          Thank you for reaching out. I'll get back to you soon.
        </p>
      </div>
    {:else}
      <form 
        on:submit|preventDefault={handleSubmit}
        class="space-y-6 bg-white dark:bg-gray-800 p-8 rounded-lg shadow-lg"
      >
        {#if error}
          <div class="bg-red-100 dark:bg-red-900 p-4 rounded text-red-900 dark:text-red-100">
            {error}
          </div>
        {/if}

        <div>
          <label 
            for="name"
            class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
          >
            Name
          </label>
          <input
            type="text"
            id="name"
            bind:value={name}
            required
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-md focus:ring-2 focus:ring-blue-500 dark:focus:ring-blue-400 focus:border-transparent bg-white dark:bg-gray-700 text-gray-900 dark:text-white"
          />
        </div>

        <div>
          <label 
            for="email"
            class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
          >
            Email
          </label>
          <input
            type="email"
            id="email"
            bind:value={email}
            required
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-md focus:ring-2 focus:ring-blue-500 dark:focus:ring-blue-400 focus:border-transparent bg-white dark:bg-gray-700 text-gray-900 dark:text-white"
          />
        </div>

        <div>
          <label 
            for="message"
            class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
          >
            Message
          </label>
          <textarea
            id="message"
            bind:value={message}
            required
            rows="6"
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-md focus:ring-2 focus:ring-blue-500 dark:focus:ring-blue-400 focus:border-transparent bg-white dark:bg-gray-700 text-gray-900 dark:text-white resize-none"
          ></textarea>
        </div>

        <button
          type="submit"
          disabled={isSubmitting}
          class="w-full bg-blue-600 hover:bg-blue-700 text-white font-medium py-3 px-6 rounded-md transition-colors duration-200 disabled:opacity-50 disabled:cursor-not-allowed"
        >
          {#if isSubmitting}
            <span class="inline-block animate-spin mr-2">⟳</span>
            Sending...
          {:else}
            Send Message
          {/if}
        </button>
      </form>
    {/if}
  </div>
</main> 