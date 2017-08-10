<template>
  <div id="app">
    <h1>Compare</h1>
    <home></home>
    <footer>
      created by Chance Smith @SodiumHalogen
    </footer>
  </div>
</template>

<script>
import Home from './components/Home'

export default {
  name: 'app',
  components: {
    Home
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
footer {
  position: absolute;
  text-align: center;
  width: 100%;
  bottom: 10px;
}
</style>
