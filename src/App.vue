<template>
  <!--img alt="Vue logo" src="./assets/logo.png"-->
  <HelloWorld msg="Vue Test"/>
  <div>
    <button class="btn btn-primary" @click="toggleStocks">Magnificent 7</button>
  </div>
  <!--BarChart /-->
  <BarChart2 v-if="showStocks" />
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
  },
  data() {
    return {
      showStocks: false,
    };
  },
  methods: {
    toggleStocks() {
      this.showStocks = !this.showStocks;
    },
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
