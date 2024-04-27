<template>
  <NuxtLayout>
    <v-app>
      <Nav />
      <NuxtPage />
    </v-app>
  </NuxtLayout>
</template>
<script setup>
</script>
<style>
.page-enter-active,
.page-leave-active {
  transition: all 0.2s;
}

.page-enter-from,
.page-leave-to {
  opacity: 0;

}
</style>