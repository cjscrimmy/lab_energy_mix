<template>
    <div>
        <h2>{{chartOptions.chart.title}}</h2>
        <GChart
          type="PieChart"
          :data="populations"
          :options="chartOptions"
        />
        <h2>Areas over 5 Million Units</h2>
         <GChart
          type="PieChart"
          :data="areas"
          :options="chartOptions"
        />
    </div>
</template>

<script>
export default {
    name: 'populations',
    data: function () {
        return {
            populations:[],
            areas: [],
            chartOptions: {
            chart: {
            title: 'Populations By Country Over 100 Million',
            subtitle: 'pie',
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
                width: "50%"
            }
        }
    },
    mounted: function () {
            fetch('https://restcountries.eu/rest/v2/all?fields=name;population')
                .then(res => res.json())
                .then(data => {
                    const origArray = data;
                    let result = [];
                    for (const object of origArray) {
                        if (object['population'] > 100000000) {
                            let array = [object['name'], object['population']];
                            result.push(array);
                        }
                    }
                    result.unshift(['Country', 'Population']);
                    this.populations = result;
                });
            fetch('https://restcountries.eu/rest/v2/all?fields=name;area')
                .then(res => res.json())
                .then(data => {
                    const origArray = data;
                    let result = [];
                    for (const object of origArray) {
                        if (object['area'] > 5000000) {
                            let array = [object['name'], object['area']];
                            result.push(array);
                        }
                    }
                    result.unshift(['Country', 'Area']);
                    this.areas = result;
                });
        }
    }
</script>

<style>

</style>