<template>
  <BlogPosts v-slot="{posts}">
      <ul>
        <li v-for="post in posts" :key="post._path">
          <NuxtLink :to="post._path" class="column hover:bg-gray-200 dark:hover:bg-gray-700">
            {{ post.title }}
          </NuxtLink>
        </li>
      </ul>
  </BlogPosts>
</template>