<template>
  <div id="app"><home-for-elevators /></div>
</template>

<script>
import HomeForElevators from "./components/Home/HomeForElevators.vue";

export default {
  name: "App",
  components: { HomeForElevators },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
