<template>
  <div id="app">
    <Films />
  </div>
</template>

<script>
import Films from "./components/Films";
export default {
  name: "App",
  components: {
    Films,
  },
};
</script>

<style>
[v-cloak] {
  display: none;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 2em;
}
</style>
