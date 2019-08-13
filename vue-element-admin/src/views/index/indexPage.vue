<template>
    <div class="container">
        <baidu-map class="map" :center="center" :zoom="zoom" :scroll-wheel-zoom="true">
            <bm-map-type :map-types="['BMAP_NORMAL_MAP', 'BMAP_HYBRID_MAP']"
                         anchor="BMAP_ANCHOR_TOP_LEFT"></bm-map-type>
            <bm-geolocation anchor="BMAP_ANCHOR_BOTTOM_RIGHT"
                            :showAddressBar="true"
                            :autoLocation="true"
                            @locationSuccess="getLocation"></bm-geolocation>
        </baidu-map>
    </div>
</template>

<script>
    import Vue from 'vue'
    import BaiduMap from 'vue-baidu-map'

    Vue.use(BaiduMap, {
        ak: 'eDIBAgIhBgG0e8r53I6Gwj9whAS5lK3M'    //这个地方是官方提供的ak密钥
    })
    export default {
        name: 'indexPage',
        components: {},
        created() {
        },
        data() {
            return {
                center: { lng: 106.49572528291, lat: 29.629146355636 },
                zoom: 20
            }
        },
        methods: {
            getLocation(po) {
                console.log(po)
            }
        }
    }
</script>

<style lang="scss" scoped>
    .container {
        height: 100%;
        padding: 10px;
        background-color: rgb(240, 242, 245);
        position: relative;
    }

    .map {
        height: 100%;
        padding: 4px;
        border-radius: 3px;
        border: 1px solid #e3e3e3;
    }
</style>
