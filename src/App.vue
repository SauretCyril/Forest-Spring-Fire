<template>
  <div id="app">
    <ForestFireSimulation />
  </div>
</template>

<script>
import ForestFireSimulation from './components/ForestFireSimulation.vue';

export default {
  name: 'App',
  components: {
    ForestFireSimulation,
  },
};
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
