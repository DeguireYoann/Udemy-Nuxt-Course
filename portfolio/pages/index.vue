<template>
<div>
Hello
</div>
</template>

<script setup>
useSeoMeta({
  title: 'Main Page',
  description: 'Welcome to this amazing portfolio',
  ogTitle: 'Main Page',
  ogDescription: '[og:description]',
  ogImage: '[og:image]',
  ogUrl: '[og:url]',
})

useHead({
  htmlAttrs: {
    lang: 'en'
  },
  link: [
    {
      rel: 'icon',
      type: 'image/png',
      href: '/favicon.png'
    }
  ]
})
</script>