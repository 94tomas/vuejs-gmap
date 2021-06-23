<template>
    <div>
        <h1>HomePage del mapa</h1>

        <div class="k-wrap">
            <div class="k-col-8">
                <h3>Click en el mapa</h3>
                <!-- Aqui ponemos en componente -->
                <google-map
                    :center="{lat: -19.5757738, lng: -65.7591459}"
                    :zoom="5"
                    @result-click="resultClickMap"
                ></google-map>
            </div>
            <div class="k-col-4">
                <h3>Resultado</h3>
                <ul>
                    <li><strong>Dirección:</strong> {{ resultMap.address }}</li>
                    <li><strong>Latitud:</strong> {{ resultMap.lat }}</li>
                    <li><strong>Longitud:</strong> {{ resultMap.lng }}</li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import GoogleMap from '@/components/GoogleMap.vue'
export default {
    components: {
        GoogleMap
    },
    data() {
        return {
            resultMap: {}
        }
    },
    methods: {
        resultClickMap(location) {
            // console.log(location);
            this.resultMap = location
        },
    },
}
</script>
<style lang="scss">
.k-wrap {
    text-align: left;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-wrap: wrap;
    flex-wrap: wrap;
    // margin-right: -15px;
    // margin-left: -15px;
    .k-col-8, .k-col-4 {
        position: relative;
        width: 100%;
        padding-right: 15px;
        padding-left: 15px;
    }
    .k-col-8 {
        -ms-flex: 0 0 66.666667%;
        flex: 0 0 66.666667%;
        max-width: 66.666667%;
    }
    .k-col-4 {
        -ms-flex: 0 0 33.333333%;
        flex: 0 0 33.333333%;
        max-width: 33.333333%;
    }
}
</style>
