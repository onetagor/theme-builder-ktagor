<template>
  <ThemeBuilder />
</template>

<script setup>
import ThemeBuilder from './components/ThemeBuilder.vue'
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}

#app {
  width: 100%;
  min-height: 100vh;
  margin: 0;
  padding: 0;
}
</style>
