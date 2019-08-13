<template>
    <div class="dashboard-container">
        <el-row :gutter="20">
            <el-col :span="8">
                <div class="grid-content">
                    <vehicle-bar class="e-chart"/>
                </div>
            </el-col>
            <el-col :span="8">
                <div class="grid-content">
                    <product-bar class="e-chart"/>
                </div>
            </el-col>
            <el-col :span="8">
                <div class="grid-content">
                    <product-pie class="e-chart"/>
                </div>
            </el-col>
            <el-col :span="8">
                <div class="grid-content">
                    <manufacturer-table/>
                </div>
            </el-col>
            <el-col :span="8">
                <div class="grid-content">
                    <chassis-pie class="e-chart"/>
                </div>
            </el-col>
            <el-col :span="8">
                <div class="grid-content">
                    <special-one-table/>
                </div>
            </el-col>
            <el-col :span="8">
                <div class="grid-content">
                    <raddar-chart/>
                </div>
            </el-col>
        </el-row>
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

    export default {
        name: 'goodsPage',
        data() {
            return {}
        },
        computed: {},

        components: {
            elRow: Row,
            elCol: Col,
            VehicleBar,
            ProductBar,
            ProductPie,
            ManufacturerTable,
            ChassisPie,
            SpecialOneTable,
            RaddarChart
        }
    }
</script>

<style lang="scss" scoped>
    .dashboard-container {
        padding: 20px;
        background-color: #F0F2F5;
    }

    .grid-content {
        height: 360px;
        margin-bottom: 20px;
        padding: 20px;
        background-color: #FFFFFF;

        .e-chart {
            width: 100%;
            height: 100%;
        }
    }
</style>
