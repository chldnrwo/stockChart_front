<template>
  <!--img alt="Vue logo" src="./assets/logo.png"-->
  <HelloWorld msg="Vue Test"/>
  <div>
    <button class="btn btn-primary btn-sm" @click="toggleStocks">Magnificent 7 YOY</button>
  </div>
  <br>
    <AutoComplete @ticker-selected="handleTickerSelected" />
  <br>
    <StockDetails v-if="selectedTicker" :ticker="selectedTicker" />
    <BarChart2 v-if="showStocks" />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';
import BarChart2 from './components/BarChart2.vue';
import AutoComplete from './components/AutoComplete.vue';
import StockDetails from './components/StockDetails.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
    BarChart2,
    AutoComplete,
    StockDetails
  },
  data() {
    return {
      showStocks: false,
      selectedTicker: '', // 선택된 티커를 저장할 변수
    };
  },
  methods: {
    toggleStocks() {
      this.showStocks = !this.showStocks;
    },
    handleTickerSelected(ticker) {
      console.log(ticker);
      this.selectedTicker = ticker; // 선택된 티커를 저장
    }
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
