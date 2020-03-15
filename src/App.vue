<template>
  <div id="app">
    <Tree></Tree>
  </div>
</template>

<script>
import Tree from './components/tree/index'

export default {
  name: 'App',
  components: {
      Tree
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
