<template>
  <div id="app">
    <h1>UK Energy Mix</h1>
    <h4>A breakdown of the UK's current energy mix, tying togther National Grid's <a href="https://carbonintensity.org.uk/">Carbon Intensity API</a> and <a href="https://developers.google.com/chart/">Google Charts</a></h4>
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

* {
  margin: 0;
  padding: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 90vh;
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

h1 {
  margin: 1vw;
}

h4 {
  margin-top: 0;
}

a {
  color: #3366CC;
}

a:hover {
  color: #FF9900;
}

</style>
