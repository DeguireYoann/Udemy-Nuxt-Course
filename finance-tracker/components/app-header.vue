<template>
  <header class="flex justify-between items-center mt-10">
    <NuxtLink to="/" class="text-xl font-bold">Finance Tracker</NuxtLink>
    <div class="flex items-center border p-3 space-x-2 border-gray-200 dark:border-gray-800">
    <UAvatar
        src="https://avatars.githubusercontent.com/u/739984?v=4"
        alt="Avatar"
    />
    <ClientOnly>
      <ColorModeSelector />
    </ClientOnly>
    </div>
  </header>
</template>