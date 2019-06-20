<template>
  <div id="app">
    <h1>Energy Mix</h1>
    <energy-chart :energyData='generationMixChartData'/>
  </div>
</template>

<script>
import EnergyChart from '@/components/EnergyChart.vue'

export default {
  name: 'app',
  data() {
    return {
      generationMix: null,
    }
  },
  components: {
    'energy-chart': EnergyChart
  },
  mounted() {
    this.fetchEnergyData()
  },
  computed:{
    generationMixChartData: function() {
      if (this.generationMix) {
        const result = this.generationMix
        .map(type => [type.fuel, type.perc])
        .sort((a, b) => -(a[1] - b[1]))
        result.unshift(['Fuel', 'Percentage']);
        return result;
      }
    }
  },
  methods: {
    fetchEnergyData: function() {
      fetch('https://api.carbonintensity.org.uk/generation')
      .then(response => response.json())
      .then(apiResult => this.generationMix = apiResult.data.generationmix);
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
