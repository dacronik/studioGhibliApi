<template>
  <div id="app">
    <Peliculas/>
  </div>
</template>

<script>
import Peliculas from "@/components/Peliculas.vue"

export default {
  name: 'App',
  components: {
    Peliculas
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
