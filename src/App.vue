<script setup>
import MemoryGame from './components/MemoryGame.vue'
</script>

<template>
  <div class="app-container">
    <MemoryGame />
  </div>
</template>

<style>
/* Global resets or generic app styles */
body {
  margin: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f7f9fc;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.app-container {
  width: 100%;
}
</style>
