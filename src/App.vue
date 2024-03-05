<script setup lang="ts">
import { RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue'
</script>

<template>
  <header>
    <Navbar />
  </header>

  <div class="container py-4 px-3 mx-auto">
    <RouterView />
  </div>
</template>

<style scoped>
</style>
