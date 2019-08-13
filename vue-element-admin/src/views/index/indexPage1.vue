<template>
    <div class="container">
        <baidu-map class="map" :center="center" :zoom="zoom" :scroll-wheel-zoom="true">
            <bm-map-type :map-types="['BMAP_NORMAL_MAP', 'BMAP_HYBRID_MAP']"
                         anchor="BMAP_ANCHOR_TOP_LEFT"></bm-map-type>
            <bm-geolocation anchor="BMAP_ANCHOR_BOTTOM_RIGHT"
                            :showAddressBar="true"
                            :autoLocation="true"
                            @locationSuccess="getLocation"></bm-geolocation>
            <bm-marker v-for="(item,index) of markedList"
                       :position="item"
                       :key="index"
                       @click="infoWindowOpen"
                       animation="BMAP_ANIMATION_BOUNCE">
                <bm-label :content="`仓库${index+1}`"
                          :offset="{width: -5, height: -40}"/>
            </bm-marker>

            <bm-driving
                :start="markedList[0]"
                :end="markedList[1]"
                :auto-viewport="true"
                @searchcomplete="handleSearchComplete"></bm-driving>

            <bm-driving
                :start="markedList[0]"
                :end="markedList[2]"
                :auto-viewport="true"></bm-driving>

            <bm-driving
                :start="markedList[1]"
                :end="markedList[2]"
                :auto-viewport="true"></bm-driving>
            <bml-lushu
                @stop="reset"
                :path="path"
                :icon="icon"
                :play="play"
                :autoView="true"
                :rotation="true"
                :speed="1000">
            </bml-lushu>
        </baidu-map>
        <map-tips/>
    </div>
</template>

<script>
  import Vue from 'vue'
  import BaiduMap from 'vue-baidu-map'
  import MapTips from './components/mapTips'
  import { BmlLushu } from 'vue-baidu-map'

  Vue.use(BaiduMap, {
    ak: 'eDIBAgIhBgG0e8r53I6Gwj9whAS5lK3M'    //这个地方是官方提供的ak密钥
  })
  export default {
    name: 'indexPage',
    components: { MapTips, BmlLushu },
    created() {
    },
    data() {
      return {
        // 标记点列表
        markedList: [{
          lng: 106.48372528291, lat: 29.631146355636
        }, {
          lng: 106.49572528291, lat: 29.629146355636
        }, {
          lng: 106.48572528291, lat: 29.622146355636
        }],
        // 初始位置
        center: { lng: 106.49572528291, lat: 29.629146355636 },
        // 地图缩放比例
        zoom: 15,
        play: true,
        path: [],
        icon: {
          url: 'http://api.map.baidu.com/library/LuShu/1.2/examples/car.png',
          size: { width: 52, height: 26 },
          opts: { anchor: { width: 27, height: 13 } }
        }
      }
    },
    methods: {
      handleSearchComplete(res) {
        this.path = res.getPlan(0).getRoute(0).getPath()
        console.log(this.path);
      },
      reset() {
        this.play = false
      },
      infoWindowClose() {
        this.show = false
      },
      infoWindowOpen() {
        this.show = true
      },
      getLocation(po) {
        console.log(po)
      },
      handleClose() {
        console.log('抽屉被关闭')
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
