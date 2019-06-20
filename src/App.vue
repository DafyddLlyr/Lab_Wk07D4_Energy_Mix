<template>
  <div id="app">
    <h1>Energy Mix</h1>
    <energy-chart />
  </div>
</template>

<script>
import EnergyChart from '@/components/EnergyChart.vue'

export default {
  name: 'app',
  data() {
    return {
      generationMix: null,
      generationMixChartData: null
    }
  },
  components: {
    'energy-chart': EnergyChart
  },
  mounted() {
    this.fetchEnergyData()
  },
  methods: {
    fetchEnergyData: function() {
      fetch('https://api.carbonintensity.org.uk/generation')
      .then(response => response.json())
      .then(apiResult => {
        const generationMix = apiResult.data.generationmix;
        this.generationMix = generationMix
        this.generationMixChartData =
          generationMix.map(type => [type.fuel, type.perc]);
        this.generationMixChartData.unshift(['Fuel', 'Percentage']);
      }
    )


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
