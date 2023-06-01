<script setup>
import { RouterLink, RouterView } from "vue-router";
</script>

<template>
  <header>
    <nav class="text-center py-10">
      <RouterLink class="px-4" to="/">Home</RouterLink>
      <RouterLink class="px-4" to="/about">About</RouterLink>
    </nav>
  </header>

  <RouterView />
</template>
