<template>
  <!--img alt="Vue logo" src="./assets/logo.png"-->
  <HelloWorld msg="Vue Test"/>
  <div>
    <button class="btn btn-primary btn-sm" @click="toggleStocks">Magnificent 7 YOY</button>
  </div>
  <div class="container mt-5">
    <form>
      <div class="row justify-content-center">
        <!-- 간격을 위해 20%씩 양옆에 col 추가 -->
        <div class="col-2"></div>
        <div class="col-8">
          <div class="row align-items-center">
            <div class="col-3 d-flex align-items-center justify-content-end">
              <label for="exampleInput" class="form-label mb-0">TICKER</label>
            </div>
            <div class="col-5">
              <input type="text" class="form-control" id="exampleInput" placeholder="Enter Ticker">
            </div>
            <div class="col-2">
              <button type="submit" class="btn btn-primary">Submit</button>
            </div>
          </div>
        </div>
        <div class="col-2"></div>
      </div>
    </form>
  </div>
  <br>

  <AutoComplete @company-selected="fetchStockData" />
  <br>

  <!--BarChart /-->
  <BarChart2 v-if="showStocks" />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
//import BarChart from './components/BarChart.vue';
import BarChart2 from './components/BarChart2.vue';
import AutoComplete from './components/AutoComplete.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
    //BarChart,
    BarChart2,
    AutoComplete
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
