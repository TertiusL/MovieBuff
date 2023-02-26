<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/MovieBuff.jpg" id="logo">
    <MovieBuff msg="Welcome to Movie Buff!"/>
  </div>
</template>

<script>
import MovieBuff from './components/MovieBuff.vue'

export default {
  name: 'App',
  components: {
    MovieBuff
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
#logo{
  width: 15%;
  height: 15%;
}
</style>
