<template>
    <div>
        <h2>Top 5 country/ regions with clicks</h2>
        <div class="map-container">
            <!-- The empty world map goes here -->
            <div id="world-map" ref="worldMap"></div>
        </div>
        <div class="country-bars">
            <div v-for="country in countries" :key="country.name" class="country-bar">
                <div class="bar" :style="{ width: country.clicks + '%' }"></div>
                <span class="country-name" :class="{ 'highlighted': country.highlighted }">{{ country.name }}</span>
                <span class="clicks-count">{{ country.clicks }}</span>
            </div>
        </div>
    </div>
</template>
  
<script>
import L from 'leaflet';

export default {
    data() {
        return {
            countries: [
                { name: 'Malaysia', clicks: 20, highlighted: false },
                { name: 'China', clicks: 15, highlighted: false },
                { name: 'Indonesia', clicks: 10, highlighted: false },
                { name: 'Brazil', clicks: 5, highlighted: false },
                { name: 'United States', clicks: 2, highlighted: false }
            ]
        };
    },
    mounted() {
        // Initialize the world map and highlight the top 5 countries
        this.initializeMap();
        this.highlightTopCountries();
    },
    methods: {
        initializeMap() {
            // Create a Leaflet map instance
            const map = L.map(this.$refs.worldMap).setView([0, 0], 2);

            // Add a tile layer to the map
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors',
                maxZoom: 18
            }).addTo(map);
        },
        highlightTopCountries() {
            // Highlight the top 5 countries on the map
            const topCountries = this.countries.slice(0, 5);

            // Iterate through the countries and set the 'highlighted' property to true
            topCountries.forEach((country) => {
                country.highlighted = true;
            });
        }
    }
};
</script>
  
<style>
.map-container {
    position: relative;
}

.country-bars {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 10px;
}

.country-bar {
    display: flex;
    align-items: center;
}

.bar {
    width: 100px;
    height: 10px;
    background-color: blue;
    margin-right: 5px;
}

.country-name {
    margin-right: 5px;
}

.clicks-count {
    font-weight: bold;
}

.highlighted {
    color: red;
}
</style>
  