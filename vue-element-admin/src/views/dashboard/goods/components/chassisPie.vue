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
                    title: {
                        text: '进口/国产底盘占国产消防车比例',
                        x: 'left'
                    },
                    tooltip: {
                        trigger: 'item',
                        formatter: '{a} <br/>{b}: {c} ({d}%)'
                    },
                    legend: {
                        left: 'center',
                        bottom: '10',
                        data: ['国产类', '进口类']
                    },
                    calculable: true,
                    series: [
                        {
                            name: '进口/国产底盘占国产消防车比例',
                            type: 'pie',
                            radius: ['50%', '70%'],
                            center: ['55%', '50%'],
                            data: [
                                { value: 34, name: '国产类' },
                                { value: 66, name: '进口类' }
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
