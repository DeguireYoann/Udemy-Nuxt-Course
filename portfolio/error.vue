<template>
  <NuxtLayout>
    <section class="flex flex-col items-center justify-between p-5">
      <span class="text-8xl mb-16">___*( ￣皿￣)/#___</span>
      <h1 class="text-4xl mb-4">(* ￣︿￣) Ooops {{ error.statusCode }} щ(゜ロ゜щ)</h1>
      <h2 class="text-xl">
        {{ error.statusMessage }}
      </h2>
    </section>
  </NuxtLayout>
</template>

<script setup>
defineProps({error: Object})
</script>