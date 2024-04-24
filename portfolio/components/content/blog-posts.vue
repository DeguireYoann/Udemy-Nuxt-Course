<template>
  <slot :posts="posts">
    <section class="not-prose font-mono w-max	">
      <div class="column text-gray-500 dark:text-lime-600 text-sm">
        <div>date</div>
        <div>title</div>
      </div>
      <ul>
        <li v-for="post in posts" :key="post._path">
          <NuxtLink :to="post._path" class="column group hover:bg-gray-200 dark:hover:bg-gray-700">
            <div
                :class="{'text-gray-200 group-hover:text-gray-400 dark:group-hover:text-lime-800 dark:text-lime-800' : !post.displayYear,  'text-gray-400 dark:text-lime-500' : post.displayYear}">
              {{ post.year }}
            </div>
            <div class="dark:text-lime-300">{{ post.title }}</div>
          </NuxtLink>
        </li>
      </ul>
    </section>
  </slot>
</template>

<script setup>
const props = defineProps({
  limit: {
    type: Number,
    default: null
  }
})
const {data} = await useAsyncData(
    'blog-list',
    () => {
      const query = queryContent('/blog')
          .where({_path: {$ne: "/blog"}})
          .sort({date: -1})
          .only(['_path', 'title', 'date'])
      if (props.limit) {
        query.limit(props.limit)
      }
      return query.find()
    }
);

const posts = computed(() => {
  if (!data.value) {
    return []
  }
  let lastYear = null;
  for (const post of data.value) {
    const year = new Date(post.date).getFullYear();
    post.displayYear = year !== lastYear;
    post.year = year;
    lastYear = year;
  }
  return data.value
});
</script>

<style scoped>
.column {
  @apply flex items-center space-x-8 py-2 border-b border-gray-200 dark:border-gray-700;
}
</style>