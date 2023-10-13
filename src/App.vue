<template>
  <div id="app">
    <crypto-price @data-updated="updateChartData" />
    <crypto-chart :euroData="euroDataList" :usdData="usdDataList" />
  </div>
</template>

<script>
import CryptoPrice from './components/CryptoPrice.vue';
import CryptoChart from './components/CryptoChart.vue';

export default {
  name: 'App',
  components: {
    CryptoPrice,
    CryptoChart,
  },
  data() {
    return {
      euroDataList: [],
      usdDataList: [],

    };
  },
  methods: {
    updateChartData(euroData, usdData) {
      this.euroDataList = euroData;
      this.usdDataList = usdData;
    }

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
