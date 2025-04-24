<template>
  <div id="app">
    <PomoDoro />
  </div>
</template>

<script>
import PomoDoro from './components/PomoDoro.vue';

export default {
  name: 'App',
  components: {
    PomoDoro
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: linear-gradient(to right, #4facfe 0%, #f6d365 100%);
  width: 100vw;
  height: 100vh;
}
</style>
