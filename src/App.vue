<template>
  <div id="app">
    <div id="logo">
      <h1>ComPear.JS</h1>
      <p>Fruitful decision sharing.</p>
    </div>
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
h1 {
  margin: 0;
}
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
