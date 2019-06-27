<template lang="html">
  <div class='container'>
    <h2>{{ chartTitle }}</h2>
    <GChart class="chart" type="PieChart" :data="chartEnergyData" :options="chartOptions" />
  </div>

</template>

<script>
export default {
  name: 'energy-chart',
  props: ['chartTitle', 'from', 'to', 'custom'],
  data() {
    return {
      title: null,
      chartOptions: {
        legend: {
          alignment: 'center',
          width: 10
        },
        title: {
          alignment: 'center'
        },
        chartArea: {
          width: '90%',
          height: '90%',
          left: '10%',
        }
      },
      energyData: [
        {fuel: 'biomass', perc: 0},
        {fuel: 'coal', perc: 0},
        {fuel: 'imports', perc: 0},
        {fuel: 'gas', perc: 0},
        {fuel: 'nuclear', perc: 0},
        {fuel: 'other', perc: 0},
        {fuel: 'hydro', perc: 0},
        {fuel: 'solar', perc: 0},
        {fuel: 'wind', perc: 0}
      ]
    }
  },
  mounted() {
    this.fetchEnergyData(this.from, this.to)
  },
  methods: {
    fetchEnergyData(from = "", to= "") {
      const url = 'https://api.carbonintensity.org.uk/generation'
      fetch(url + from + to)
      .then(response => response.json())
      .then(apiResult => {
        if (!apiResult.data.length) {
          this.energyData = apiResult.data.generationmix
        } else {
          let apiData = {}
          apiData = apiResult.data.map(halfHour => halfHour.generationmix)
          this.energyData.forEach(fuelType => {
            apiData.forEach(halfHour => {
              halfHour.forEach(fuel => {
                if (fuel['fuel'] === fuelType['fuel']) {
                  fuelType['perc'] += fuel['perc'];
                }
              })
            })
          })
        }
      })
    }
  },
  computed:{
    chartEnergyData: function() {
      if (this.energyData) {
        const result = this.energyData
        .map(type => [type.fuel, type.perc])
        .sort((accumulator, type) =>  -(accumulator[1] - type[1]))
        result.unshift(['Fuel', 'Percentage']);
        return result;
      }
    }
  }
}
</script>

<style lang="css" scoped>

h2 {
  margin: 0;
  padding: 0;
}

.chart {
  width: 100%;
  height: 100%;
}

.container {
  margin: 2vw;
}

.date-input {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  margin-top: 2vw;
}

.input-wrapper {
  margin: 0 2vw;
}

input {
  font-size: inherit;
  font-family: inherit;
}

</style>
