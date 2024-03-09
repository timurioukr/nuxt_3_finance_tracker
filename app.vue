<template>
  <NuxtLayout>
    <NuxtPage />
    <UNotifications />
  </NuxtLayout>
</template>

<script setup>

const theme = onMounted(() => {
    document.documentElement.classList.add('dark');
  
})

</script>