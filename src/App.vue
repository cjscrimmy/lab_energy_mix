<template>
  <div>
    <h1> {{ chartOptions.chart.title }} </h1>
    <GChart
      type="ColumnChart"
      :data="energy_mix"
      :options="chartOptions"
    />
  </div>
</template>

<script>
export default {
  name: 'app',
  data: function () {
    return {
      energy_mix: [],
      chartOptions: {
        chart: {
          title: 'Energy Mix',
          subtitle: 'Yay!',
        }
      }
    }
  
  },
  mounted: function () {
    fetch('https://api.carbonintensity.org.uk/generation')
    .then(res => res.json())
    .then(generation => {
      const origArray = generation.data.generationmix;
      let result = [];
      for (const object of origArray) {
         let array = [object['fuel'], object['perc']];
         result.push(array);
      }
      this.energy_mix = result;
      this.energy_mix.unshift(['Fuel', 'Percentage']);
    });
  },
}
</script>

<style>

</style>
