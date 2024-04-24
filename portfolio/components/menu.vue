<template>
    <nav class="font-mono">
        <ul class="flex flex-col md:flex-row md:space-x-4">
          <li>
            <NuxtLink to="/" class="link">main ಠ﹏ಠ</NuxtLink>
          </li>
          <li>
            <NuxtLink class="link" to="/about">about (°ー°〃)</NuxtLink>
          </li>
          <li>
            <NuxtLink class="link" to="/projects">projects ┗|｀O′|┛</NuxtLink>
          </li>
          <li>
            <NuxtLink class="link" to="/blog">blog（⊙ｏ⊙）</NuxtLink>
          </li>
        </ul>
      </nav>
</template>

<style scoped>
.link {
    @apply p-1 hover:bg-gray-200 dark:hover:bg-gray-700 text-2xl md:text-base;
}
</style>