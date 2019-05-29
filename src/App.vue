<template>
  <div id="app">
    <tree-table-basic></tree-table-basic>
    <hr>
    <tree-table-lazy></tree-table-lazy>
  </div>
</template>

<script>
import TreeTableBasic from './views/demo/tree-table-basic'
import treeTableLazy from './views/demo/tree-table-lazy'

export default {
  name: 'app',
  components: {
    TreeTableBasic,
    treeTableLazy
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
