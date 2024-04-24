<template>
  <article class="content">
    <ContentDoc>
      <template #not-found>
        <h1>Document not found</h1>
      </template>
      <template v-slot="{ doc }">
        <div class="grid grid-cols-6 gap-16">
          <div v-if="doc.toc" class="hidden md:block md:col-span-2 not-prose">
            <aside class="sticky top-8">
              <div class="font-semibold mb-2 dark:text-lime-300"> Table of Content</div>
              <nav>
                <TocLinks :links="doc?.body?.toc?.links" :activeId="activeId"/>
              </nav>
            </aside>
          </div>
          <div :class="{'col-span-6 md:col-span-4' : doc.toc, 'col-span-6' : !doc.toc}">
            <ContentRenderer :value="doc"/>
          </div>
        </div>
      </template>
    </ContentDoc>
  </article>
</template>

<script setup>
const activeId = ref(null);
onMounted(() => {
  const callback = (entries) => {
    for (let entry of entries) {
      if (entry.isIntersecting) {
        activeId.value = entry.target.id
      }
    }
  };

  const observer = new IntersectionObserver(callback, {
    root: null,
    threshold: 0.5
  })
  const elements = document.querySelectorAll('h2, h3')

  for (const element of elements) {
    observer.observe(element)
  }

  onBeforeUnmount(() => {
    for (const element of elements) {
      observer.unobserve(element);
    }
  })
})
</script>