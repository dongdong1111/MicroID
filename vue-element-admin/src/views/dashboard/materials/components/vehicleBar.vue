<template>
    <div></div>
</template>

<script>
    import echarts from 'echarts'
    import resize from '../../logistics/components/mixins/resize'

    import('echarts/theme/macarons')

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
            if (this.chart) {
                this.chart.dispose()
                this.chart = null
            }
        },
        methods: {
            initChart() {
                this.chart = echarts.init(this.$el, 'macarons')

                this.chart.setOption({
                    tooltip: {
                        trigger: 'item',
                        formatter: '{a} <br/>{b}: {c} ({d}%)'
                    },
                    // legend: {
                    //     orient: 'vertical',
                    //     x: 'left',
                    //     data: ['抢险救援车', '装备运输车', '物资运输车', '淋浴车', '住宿车', '破拆车'
                    //         , '野外炊事车', '饮食净化车', '移动供气车', '油料车', '发电应急车']
                    // },
                    calculable: true,
                    series: [
                        {
                            name: '各类消防车占比',
                            type: 'pie',
                            roseType: 'radius',
                            radius: ['50%', '70%'],
                            center: ['55%', '50%'],
                            data: [
                                { value: 3, name: '抢险救援车' },
                                { value: 6, name: '装备运输车' },
                                { value: 4, name: '物资运输车' },
                                { value: 2, name: '淋浴车' },
                                { value: 3, name: '住宿车' },
                                { value: 5, name: '破拆车' },
                                { value: 3, name: '野外炊事车' },
                                { value: 6, name: '饮食净化车' },
                                { value: 3, name: '移动供气车' },
                                { value: 2, name: '油料车' },
                                { value: 1, name: '发电应急车' }
                            ],
                            animationEasing: 'cubicInOut',
                            animationDuration: 2600
                        }
                    ]
                })
            }
        }
    }
</script>

<style lang="scss">

</style>
