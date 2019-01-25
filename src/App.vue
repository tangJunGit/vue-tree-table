<template>
  <div id="app">
    <tree-table-basic></tree-table-basic>
  </div>
</template>

<script>
import TreeTableBasic from './components/tree-table-basic'

export default {
  name: 'app',
  components: {
    TreeTableBasic
  }
}
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
