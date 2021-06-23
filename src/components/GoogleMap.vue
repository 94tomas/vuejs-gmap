<template>
    <div>
        <div id="map"></div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            map: null,
            markers: [],
            resultClick: {
                address: '',
                lat: null,
                lng: null
            }
        }
    },
    props: ['center', 'zoom'],
    mounted() {
        this.initMap()
    },
    methods: {
        initMap() {
            this.map = new window.google.maps.Map(document.getElementById("map"), {
                zoom: this.zoom,
                center: this.center,
            });
            // Mapa básica
            // this.markers = new window.google.maps.Marker({
            //     position: { lat: -28.024, lng: 140.887 },
            //     map: this.map,
            // });
            this.map.addListener('click', event => {
                this.addMarker(event.latLng)
            })
            this.addMarker(this.center)
        },
        addMarker (location) {
            // console.log(location);
            this.deleteMarkers()
            const marker = new window.google.maps.Marker({
                position: location,
                // icon: 'url',
                map: this.map
            })
            this.markers.push(marker)
            this.getLatLng(location)
        }, // Sets the map on all markers in the array.
        deleteMarkers () {
            for (let i = 0; i < this.markers.length; i++) {
                this.markers[i].setMap(null)
            }
            this.markers = []
            this.resultClick.address = ''
        },
        getLatLng (location) {
            // console.log(location);
            // geocoder
            // var myLatlngCurrent = new window.google.maps.LatLng(this.center.lat, this.center.lng)
            const geocoder = new window.google.maps.Geocoder()
            geocoder.geocode({ location: location }, (results, status) => {
                if (status === 'OK') {
                    if (results[0]) {
                        console.log(results);
                        this.resultClick.address = results[0].formatted_address
                    } else {
                        alert('Sin resultados')
                    }
                } else {
                    alert('El geocodificador falló debido a: ' + status)
                }
            })
            // obtenemos lat y lng
            if (typeof(location.lat) === 'number') {
                this.resultClick.lat = location.lat
                this.resultClick.lng = location.lng
            } else {
                const tmpLocation = location.toString()
                // (-15.54, -65.3434)
                const tmpReplace = tmpLocation.replace('(', '').replace(')', '')
                const coord = tmpReplace.split(', ')
                this.resultClick.lat = parseFloat(coord[0])
                this.resultClick.lng = parseFloat(coord[1])
            }
            this.$emit('result-click', this.resultClick)
        },
    },
}
</script>
<style lang="scss">
#map {
  height: 400px;
}
</style>
