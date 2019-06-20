<template lang="html">
  <GChart
  type="ColumnChart"
  :data="energyData"
  :options="chartOptions"
  />
</template>

<script>
export default {
  name: 'energy-chart',
  props: ['GChart', 'energyData'],
  data() {
    return {
      chartOptions: {
          title: 'UK Energy Mix'
      }
    }
  }
}
</script>

<style lang="css" scoped>
</style>
