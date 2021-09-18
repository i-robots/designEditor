<template>
    <Canvas />
</template>

<script>
import Canvas from './components/Canvas'

export default {
  name: 'App',
  components: {
    Canvas
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body{
   margin: 0;
}
</style>
