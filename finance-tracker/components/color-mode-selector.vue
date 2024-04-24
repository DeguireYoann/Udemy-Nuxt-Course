<template>
    <div class="flex space-x-2 items-center group">
        <div class="text-gray-500 text-xs invisible group-hover:visible">{{ currentMode }}</div>
        <UToggle color="primary" v-model="isDarkMode" />
    </div>
  </template>
  
  <script setup>
  const colorMode = useColorMode()
  const isDarkMode = ref(colorMode.preference === 'dark')
  const currentMode = computed(() => {
    const mode = isDarkMode.value ? 'dark' : 'light'
    colorMode.preference = mode;
    return mode
  })
  </script>