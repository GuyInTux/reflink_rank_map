<template>
    <div id="app">
        <vue-datamaps>
            :fills="fills"
            :data="data"
            :geography-config="geographyConfig"
            bubbles
            :bubbles-config="bubblesConfig"
        </vue-datamaps>
        <!-- <VueDatamaps :options="mapOptions" :fills="mapFills" :data="mapData" /> -->
    </div>
</template>

<script>
import { VueDatamaps } from 'vue-datamaps';

export default {
    name: 'App',
    components: {
        VueDatamaps
    },
    data() {
        return {
            mapOptions: {
                geographyConfig: {
                    popupOnHover: true,
                    highlightOnHover: true,
                    highlightFillColor: '#FC8D59',
                    highlightBorderColor: '#000000',
                    highlightBorderWidth: 2,
                    popupTemplate: function (geography) {
                        return `<div class="hoverinfo"><strong>${geography.properties.name}</strong></div>`;
                    }
                },
                done: (datamap) => {
                    datamap.svg.selectAll('.datamaps-subunit').on('mouseenter', function (geography) {
                        clearTimeout(datamap.options.hoverDelay);
                        datamap.options.hoverDelay = setTimeout(() => {
                            datamap.updatePopup(this, geography, datamap.options.geographyConfig.popupTemplate(geography, geography));
                        }, 500); // 500ms delay
                    });
                }
            },
            mapFills: {},
            mapData: {}
        }
    }
}
</script>
