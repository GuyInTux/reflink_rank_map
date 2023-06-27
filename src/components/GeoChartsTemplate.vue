<template>
    <div id="app">
        <GChart :settings="{ packages: ['corechart', 'geochart'] }" type="GeoChart" :data="chartData"
            :options="chartOptions" :events="chartEvents" />
    </div>
</template>
  
<script>
import { GChart } from 'vue-google-charts';

export default {
    name: 'App',
    components: {
        GChart,
    },
    data() {
        return {
            chartData: [
                ['Country Code', 'Country', 'Total Reflink Clicks'], // without html tooltip
                // ['Country Code', 'Country', 'Total Reflink Clicks', { role: 'tooltip', type: 'string', p: { 'html': true } }], // html tooltip enabled
                // ['China', 100],
                // ['Germany', 200],
                // ['United States', 300],
                // ['cn', 400],
                // ['Brazil', 500],
                // ['my', 1000],
                // ['ru', 200],
            ],
            chartEvents: {
                'select': () => {
                    parseInt(11) // event listener on region selection, placeholder for now
                }
            },
            chartOptions: {
                title: 'Reflink Clicks by Country/ Region',
                width: 1000,
                height: 800,
                colorAxis: { colors: ['red', 'red'] },
                tooltip: { isHtml: true, textStyle: { color: 'green' }, showColorCode: true },
            }

        }
    },

    created() { // Process this bfr populating into geochart
        fetch("https://staging-sls.g2g.com/locale/country")
            .then(response => response.json())
            .then(data =>
                this.processData(data)
            );
    },

    methods: {
        processData(data) { // NOTE: GeoChart expects a data structure of an array of arrays, the 1st array(header row) expects 2 to 3(optional) columns.
            const results = data.payload.results.map(result => [
                result.country, // 1st element: string to map data markers on the chart
                result.country_name, // 2nd element: number type value associated w that string marker
                parseInt(result.dial_code), // optional element, string value to describe the content
            ]);
            this.chartData.push(...results);

        }
    },
};

</script>
  
<style></style>
  