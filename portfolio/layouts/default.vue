<template>
  <div class="container mx-auto max-w-4xl">
    <header class="flex justify-between items-center mt-5">
      <div class="flex items-center space-x-8">
        <div>
          <NuxtLink to="/" class="text-xl font-semibold p-2 hover:bg-gray-200 dark:hover:bg-gray-700">Yoann Deguire</NuxtLink>
        </div>
        <Menu />
      </div>
      <ClientOnly>
        <ColorModeSelector />
      </ClientOnly>
    </header>
    <main class="p-2 mt-10">
      <slot/>
    </main>
    </div>
</template>

<script setup>
useHead({
  titleTemplate: '%s - Yoann Deguire',
  link: [
    {
        rel: 'preconnect',
        href: 'https://fonts.googleapis.com'
    },
    {
        rel: 'stylesheet',
        href: 'https://fonts.googleapis.com/css2?family=Roboto&display=swap',
        crossorigin: ''
    }
  ]
})
</script>

<style>
body {
  font-family: 'Roboto';
}

body {
  @apply bg-white dark:bg-gray-800 text-gray-700 dark:text-lime-300;
}

.content {
  @apply prose dark:prose-invert max-w-none dark:prose-a:text-lime-300 dark:text-gray-400 dark:prose-h1:text-lime-300 dark:prose-h2:text-lime-300 dark:prose-h3:text-lime-300 dark:prose-strong:text-lime-300 prose-pre:bg-white dark:prose-pre:bg-gray-800 prose-pre:text-gray-700 dark:prose-pre:text-gray-300;
}
</style>