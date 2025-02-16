<template>
  <div id="app">
   <FlipBookView/>
  </div>
</template>

<script>
import FlipBookView from "./components/FlipBookView.vue";

export default {
  name: 'App',
  components: {
    FlipBookView
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
