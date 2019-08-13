<template>
  <div />
</template>

<script>
import echarts from 'echarts'
import('echarts/theme/macarons') // echarts theme
import resize from '../../logistics/components/mixins/resize'

export default {
  mixins: [resize],
  data() {
    return {
      chart: null
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.initChart()
    })
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    this.chart.dispose()
    this.chart = null
  },
  methods: {
    initChart() {
      this.chart = echarts.init(this.$el, 'macarons')

      this.chart.setOption({
        title: {
          text: '各类消防车进口/国产占比',
          x: 'left'
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: { // 坐标轴指示器，坐标轴触发有效
            type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
          }
        },
        grid: {
          top: '12%',
          left: '7%',
          right: '5%',
          bottom: '7%'
        },
        xAxis: [{
          type: 'category',
          data: ['抢险救援车', '装备运输车', '物资运输车', '淋浴车', '住宿车', '移动供气车', '发电应急车']
        }],
        yAxis: [{
          type: 'value'
        }],
        series: [{
          name: '进口类',
          type: 'bar',
          data: [79, 52, 200, 334, 390, 330, 220]
        }, {
          name: '国产类',
          type: 'bar',
          data: [80, 52, 200, 334, 390, 330, 220]

        }]
      })
    }
  }
}
</script>
