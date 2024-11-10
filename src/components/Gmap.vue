<template>
    <div class="wrapper_component map">
        <div class="google-map" ref="googleMap"></div>
        <div class="component_container map">

            <div class="component_map_info_marker">
                <div class="map_info_marker_items" v-html="markersRealEstateContentsInfo"></div>
            </div>
        </div>
    </div>
</template>

<script>

import axios from 'axios'

export default {
    name: 'GogleMapCustom',
    props: ['google'],
    data() {
        return {
            //google: window.google,
            Gmap: null,
            myLatlng: {
                lat: 53.614,
                lng: 87.337
            },
            marker: null,
            markers: [],
            mapMarkers: [],
            infoWindow: null,
            // bounds: new this.google.maps.LatLngBounds(),
            mapOptions: {
                center: { lat: 53.604, lng: 87.337 },
                zoomControl: true,
                zoom: 14,
                disableDefaultUI: true
            },
            markersRealEstateContentsInfo: ''
        }
    },
    methods: {
        initMap() {

            console.log(this.google.maps)

            this.Gmap = new this.google.maps.Map(this.$refs.googleMap, {
                ...this.mapOptions
            });
        },
        getMarkers() {
            let url = "../assets/markers.js";

            axios.get(url)
                .then(response => {
                    console.log(response.data);
                });
        }
    },
    created() {
        this.getMarkers();
    },
    mounted() {
        console.log('Gmap mounted');
        console.log(this.google);
        this.initMap();
    }
}
</script>

<style>
.wrapper_component.map {
    display: flex;
    width: 100%;
    min-height: 100vh;
}

.google-map {
    display: block;
    width: 50%;
    height: 100%;
    min-height: 100vh;
}
</style>