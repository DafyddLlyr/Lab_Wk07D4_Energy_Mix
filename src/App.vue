<template>
  <div id="app">
    <h1>UK Energy Mix</h1>
    <div id="charts">
      <energy-chart chartTitle="Past 30 minutes"/>
      <custom-chart chartTitle="Custom Timeframe" class="custom"/>
      <energy-chart chartTitle="Past 24 hours"
        :from='"/" + timeNow + "/"'
        to="pt24h" />
    </div>
  </div>
</template>

<script>
import EnergyChart from '@/components/EnergyChart.vue'
import CustomEnergyChart from '@/components/CustomEnergyChart.vue'

export default {
  name: 'app',
  data() {
    return {
      timeNow: new Date(Date.now()).toJSON()
    }
  },
  components: {
    'energy-chart': EnergyChart,
    'custom-chart': CustomEnergyChart
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
  height: 100vh;
}

#charts {
  display: grid;
  grid-template-columns: 30vw auto;
  grid-auto-rows: auto auto;
  grid-template-areas:
   'preset custom'
   'preset custom';
}

.custom {
  grid-area: custom;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

</style>
