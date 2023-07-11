<template>
    <div id="app">
        <div id="container" style="position: relative; width: 50%; height:600px; margin-left:20%;"></div>
    </div>
</template>

<script>
import Datamap from 'datamaps';
import 'country-iso-2-to-3'; // refer to index.js in node_modules/country-iso-2-to-3 for country code conversion/ edits

export default {
    name: 'App',
    data() {
        return {
            map: null,
            countries: []
        }
    },
    mounted() {
        this.map = new Datamap({
            element: document.getElementById('container'),
            projection: 'mercator',
            geographyConfig: { // check line 20 under datamaps.js for default options
                popupOnHover: true,
                highlightOnHover: true,
                highlightFillColor: 'black',
                highlightBorderColor: 'orange',
                popupTemplate: function (geography, data) {
                    return `<div class="hoverinfo"><b>${geography.properties.name}</b><br>
                    Total Reflink Clicks:${data.clicks}</div>;`
                }
            },
            fills: { // priority: custom fill -> default fill
                TOPFIVE: 'blue',
                defaultFill: 'red'
            },
        });
        this.fetchData();
    },
    methods: {
        async fetchData() {
            try {
                const response = await fetch('https://staging-sls.g2g.com/locale/country');
                const data = await response.json();
                this.countries = data.payload.results;
                console.log("Data Fetched.")
                this.updateFills();
            }
            catch (error) {
                console.error(error);
            }
        },
        updateFills() {
            const getCountryISO3 = require('country-iso-2-to-3');
            const fills = {};
            const sortedCountries = [...this.countries].sort((a, b) => b.dial_code - a.dial_code);
            const top5Countries = sortedCountries.slice(0, 5);
            for (const country_row of top5Countries) {
                const iso3Format = getCountryISO3(country_row.country.toUpperCase()); // NOTE: .country is a key in the API response
                country_row.iso3Format = iso3Format; // adds new key-value pair to country_row object literal
                fills[country_row.iso3Format] = { fillKey: 'TOPFIVE', clicks: country_row.dial_code };
            }
            this.map.updateChoropleth(fills);
        },

    }
}
</script>

<style>
#app {
    margin-bottom: 100px;
}

#container {
    width: 100%;
    height: 100%;
}
</style>
