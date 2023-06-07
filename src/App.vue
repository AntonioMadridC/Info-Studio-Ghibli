<template>
  <div id="app">
    <ghibli-get></ghibli-get>
    
  </div>
</template>

<script>
import GhibliGet from './components/GhibliGet.vue'


export default {
  name: 'App',
  components: {
    GhibliGet
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
