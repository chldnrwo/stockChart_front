<template>
  <!--img alt="Vue logo" src="./assets/logo.png"-->
  <HelloWorld msg="Vue Test"/>
  <!--BarChart /-->
  <BarChart2 />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
//import BarChart from './components/BarChart.vue';
import BarChart2 from './components/BarChart2.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
    //BarChart,
    BarChart2
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
