<template>
  <div id="app">
    <kdemo msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import kdemo from './components/kdemo.vue'

export default {
  name: 'app',
  components: {
    kdemo
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
