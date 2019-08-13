<template>
  <div class="dashboard-container">
    <div class="data-header">
      <div class="left_1">
        <div class="i-box1">
            <!--<div style="width: 100px; height:20px; border-right: 1px solid #ccc; border-radius: 5px; background: #30B08F;"></div>-->
          <vehicle-bar class="e-chart" />
        </div>
          <div class="i-box2">
              <product-pie class="e-chart"/>
          </div>
        </div>
        <div class="data-center">
            <china-map class="e-chart"/>
        </div>
      <div class="right_1">
          <div class="i-box1">
              <chassis-pie class="e-chart"/>
          </div>
          <div class="i-box2">
              <product-bar class="e-chart"/>
          </div>
      </div>
    </div>
    <div class="data-footer">
        <div class="i-box1" >
            <vehicle-bar class="e-chart" />
        </div>
      <div class="i-box2">
        <manufacturer-table />
      </div>
      <div class="i-box3">
          <special-one-table/>
      </div>
      <div class="i-box4">
        <manufacturer-table />
      </div>
    </div>
  </div>
</template>

<script>
import { Row, Col } from 'element-ui'
import VehicleBar from './components/vehicleBar'
import ProductBar from './components/productBar'
import ProductPie from './components/productPie'
import ManufacturerTable from './components/manufacturerTable'
import ChassisPie from './components/chassisPie'
import SpecialOneTable from './components/specialOneTable'
import RaddarChart from './components/raddarChart'
import chinaMap from './components/chinaMap'

export default {
  name: 'materialsPage',
  components: {
    VehicleBar,
    ProductBar,
    ProductPie,
    ManufacturerTable,
    ChassisPie,
    SpecialOneTable,
    RaddarChart,
    chinaMap
  },
  data() {
    return {}
  },
  computed: {}
}
</script>

<style lang="scss" scoped>
    .dashboard-container {
        padding: 20px;
        background-color: #F0F2F5;
    }
    .e-chart {
        width: 100%;
        height: 100%;
    }
    .data-header{
        display: flex;
    }
    .left_1{
        width: 400px;
        height: 600px;
        .i-box1{
            height: 290px;
            background: #fff;
        }
        .i-box2{
            height: 290px;
            margin-top: 20px;
            background: #fff;
        }
    }
    .right_1{
        width: 400px;
        height: 600px;

        .i-box1{
            height: 290px;
            background: #ffffff;
        }
        .i-box2{
            height: 290px;
            margin-top: 20px;
            background: #ffffff;
        }
    }
    .data-center{
        flex: 1;
        height: 600px;
        margin-left: 20px;
        margin-right: 20px;
        background: #ffffff;
    }
    .data-footer{
        display: flex;
        .i-box1{
            width: 400px;
            height: 290px;
            background: #fff;
            margin-top: 20px;
        }
        .i-box4{
            width: 400px;
            height: 290px;
            margin-top: 20px;
            background: #ffffff;
            }
        .i-box2{
            flex: 1;
            height: 290px;
            margin: 20px;
            background: #ffffff;
        }
        .i-box3{
            flex: 1;
            height: 290px;
            margin-top: 20px;
            margin-right: 20px;
            background: #ffffff;
        }
    }
</style>
