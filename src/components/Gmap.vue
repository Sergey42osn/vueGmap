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
import { MarkerClusterer } from "@googlemaps/markerclusterer";

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
            markersData:[],
            mapMarkers: [],
            markerCluster: null,
            infoWindow: null,
            Mapbounds: new this.google.maps.LatLngBounds(),
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

            this.Gmap.addListener("bounds_changed", () => {
                // 3 seconds after the center of the map has changed, pan back to the
                // marker.
                window.setTimeout(() => {
                    // this.Gmap.panTo(marker.position);

                    this.getMapBounds();

                }, 3000);
            });

            this.infoWindow = new this.google.maps.InfoWindow({
                            content: "",
                            disableAutoPan: true,
                        });

            // let markers = this.markers;
            // let Gmap = this.Gmap;

           this.markerCluster = new MarkerClusterer({});
        },
        getMarkers() {
            //let url = "https://localhost:8080/src/assets/markers.js";

            let url = "/js/markers.js";

            console.log(url);

            axios.get(url)
                .then(response => {
                    console.log(response.data);
                    if (response.data) {

                        this.markersData = response.data;

                        this.markers = this.markersData.map((el) => {

                            const latLng = new this.google.maps.LatLng(el.coords[0], el.coords[1]);

                            const marker = new this.google.maps.Marker({
                                position: latLng,
                                map: this.Gmap,
                            });

                            // markers can only be keyboard focusable when they have click listeners
                            // open info window when marker is clicked
                            marker.addListener("click", () => {
                                this.infoWindow.setContent(latLng.lat + ", " + latLng.lng);
                                this.infoWindow.open(this.Gmap, marker);
                            });
                            return marker;
                        });
                    }
                });
        },
        setMarkersMap() {
            console.log('setMarkers');
            console.log(this.mapMarkers);
        },
        getMarkersMap() {
            console.log('getMarkersMap');
            console.log(this.Mapbounds);
            let m = [];
            this.markers.forEach(el => {
                console.log(el.getPosition());
                if(this.Mapbounds.contains(el.getPosition())){
                    m.push(el);
                }
                this.mapMarkers = m;
            });
        },
        getMapBounds(){
            this.Mapbounds = this.Gmap.getBounds();
            this.getMarkersMap();
        }
    },
    created() {
        this.getMarkers();
    },
    mounted() {
        console.log('Gmap mounted');
        console.log(this.google);
        this.initMap();
    },
    watch: {
        mapMarkers: function () {
            this.setMarkersMap();
        }
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