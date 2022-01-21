<template>
  <div id="app">

    <CatalogCategories />
  </div>
</template>

<script>
import CatalogCategories from './components/CatalogCategories.vue'


export default {
  name: 'App',
  components: {
    CatalogCategories,
  }
}
</script>

<style>
#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
