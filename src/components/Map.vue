<template>
    <div id="map">

        <l-map
            ref="map"
            :center="center"
            v-model="zoom"
            v-model:zoom="zoom"
            :options="{zoomControl:false, attributionControl:true}"
            :minZoom="minZoom"
            
        >
        <!-- Background maps: -->
        <l-tile-layer
            v-for="tileProvider in tileProviders"
            :key="tileProvider.name"
            :name="tileProvider.name"
            :visible="tileProvider.visible"
            :url="tileProvider.url"
            :attribution="tileProvider.attribution"
            layer-type="base" 
            />

        <!-- Add geojson data -->
      

         <!-- Leaflet control tools here: -->
        <l-control-scale position="bottomright" :imperial="false" :metric="true" /> 
        <l-control-layers position="topright" />
        <l-control-zoom position="topright" />

        </l-map>
    </div>
</template>

<script>
import {
  LMap,
  LTileLayer,
  LControlLayers,
  LControlScale,
  LControlZoom,
  
} from "@vue-leaflet/vue-leaflet";
import "leaflet/dist/leaflet.css";

export default {
    name: 'map-component',
    components: {
        LMap,
        LTileLayer,
        LControlLayers,
        LControlScale,
        LControlZoom,
    },
    // props: {geojson: Object},
    data() {
        return {
            center: [34.82688, -156.05821],
            zoom: 3,
            minZoom: 2,
            geojsonOptions: {},
            circleColor:'#eb4034',
            highlightColor:'#ffff00',
            curr_data: {},
            tileProviders: [
                {
                name: 'OpenStreetMap',
                url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
                attribution:'&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
                },
                {
                name: 'ESRI_WorldImagery',
                url: 'https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',
                attribution:
                    'Tiles &copy; Esri &mdash; Source: Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community',
                },
                {
                name: 'Stamen.TerrainBackground',
                url: 'https://stamen-tiles-{s}.a.ssl.fastly.net/terrain-background/{z}/{x}/{y}{r}.png',
                attribution: 'Map tiles by <a href="http://stamen.com">Stamen Design</a>, <a href="http://creativecommons.org/licenses/by/3.0">CC BY 3.0</a> &mdash; Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
                },
                {name: 'Stamen.Watercolor',
                url: 'https://stamen-tiles-{s}.a.ssl.fastly.net/watercolor/{z}/{x}/{y}.png',
                attribution:'Map tiles by <a href="http://stamen.com">Stamen Design</a>, <a href="http://creativecommons.org/licenses/by/3.0">CC BY 3.0</a> &mdash; Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
                },
                
            ],
        };
    },
   


    async beforeMount() {
        // Load leaflet components (here, circleMarker)
      

        // And now the Leaflet circleMarker function can be used by the options:
        // this.geojsonOptions.pointToLayer = (feature, latLng) =>
        //     circleMarker(latLng, { 
        //         radius: feature.properties.mag*5,
        //         stroke: false,
        //         fillColor: this.circleColor,
        //         fillOpacity: 0.5,
        //         className: 'circle',
        //         pane: 'markerPane'
        //     });

        // this.geojsonOptions.onEachFeature = (feature, layer) => {
        //     var self = this;
        //     layer.bindTooltip(
        //             "<b>magnitude:</b> " + feature.properties.mag + "<br/>" +
        //             "<b>place:</b> " + feature.properties.place + "<br/>" +
        //             "<b>time:</b> "+ Date(feature.properties.time),{ permanent: false, sticky: true }
        //     );
        //     layer.on('mouseover', function() {
        //         layer.setStyle({fillColor: '#ffff00'})
        //     });
        //     layer.on('mouseout', function() {
        //         layer.setStyle({fillColor: '#eb4034'})
        //     });
        //     layer.on('click', function() {
        //         self.curr_data = feature.properties
        //         self.UPDATE_CURRDATA({data: self.curr_data})
                
        //     });
            
        // },

        this.mapIsReady = true;
    },

    computed: {
        markerSize(mag) {
                return mag * 25000
        },
    },
    methods: {
  
    },
}
</script>	




<style scoped>
#map {
  position: fixed;
  height: 100vh; 
  width: 100vw;
  margin: 0;
  z-index: 0;
};
.l-control-layers { 
  text-align: left; 
}

/* .circle:hover {
 fill: greenyellow;
} */


</style>