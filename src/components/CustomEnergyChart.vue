<template lang="html">
  <div class='container'>
    <h2>{{ chartTitle }}</h2>
    <div class="date-input">
      <div class="input-wrapper">
        <label for="from">From: </label>
        <input type="date" name="from" v-model="from" @change="updateEnergyData">
      </div>
      <div class="input-wrapper">
        <label for="to">To: </label>
        <input type="date" date="to" v-model="to"
        @change="updateEnergyData">
      </div>
    </div>
    <GChart class="chart" type="PieChart" :data="chartEnergyData" :options="chartOptions" />
  </div>

</template>

<script>
export default {
  name: 'custom-chart',
  props: ['chartTitle'],
  data() {
    return {
      title: null,
      to: 'pt24h',
      from: '/2019-06-26T20:28:20.985Z/',
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
      let append = from + to
      if (to != 'pt24h') { append = `/${this.from}T00:00:00.001Z/${this.to}T23:59:59.000Z`}
      const url = 'https://api.carbonintensity.org.uk/generation'
      console.log(url, append);
      fetch(url + append)
      .then(response => response.json())
      .then(apiResult => {
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
    )},
    updateEnergyData() {
      this.fetchEnergyData(this.from, this.to)
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
