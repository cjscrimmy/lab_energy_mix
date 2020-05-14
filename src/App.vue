<template>
  <div id="main-div">
    <h1> Vue Google Graphs </h1>
    <h2> {{ chartOptions.chart.subtitle }} </h2>
    <div id="piechart-3d">
        <GChart
          type="PieChart"
          :data="energy_mix"
          :options="chartOptions"
        />
    </div>
    <hr>
    <populations></populations>
  </div> <!-- end of main div -->
</template>

<script>
import Populations from './components/Populations.vue';

export default {
  name: 'app',
  data: function () {
    return {
      energy_mix: [],
      chartOptions: {
        chart: {
          title: 'Energy Mix',
          subtitle: 'Type of Energy vs Percentage of Energy Generated',
        },
        is3D: true,
        chartArea: {
          left: 0,
          right: 0,
          height: 600,
          width: 400
        },
        legend: {
          right: 0
        },
        height: 400,
        width: "100%"
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
  components: {
    'populations': Populations
  }
}
</script>

<style>
  #piechart-3d{
    padding: 40px;
    border: 1px solid black;
    width: 50%
  }
</style>
