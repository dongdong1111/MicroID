<template>
    <el-table
        :data="tableData2"
        style="width: 400px;"
        :row-class-name="tableRowClassName">
        <el-table-column
            prop="number"
            label="运营数(辆)"
            width="100">
        </el-table-column>
        <el-table-column
            prop="length"
            label="线路总长度(公里)"
            width="130">
        </el-table-column>
        <el-table-column
            prop="total"
            label="客运总量"
            width="90">
        </el-table-column>
        <el-table-column
            prop="date"
            label="日期"
            width="80">
        </el-table-column>
    </el-table>
</template>

<script>
    import { Table, TableColumn } from 'element-ui'

    export default {
        methods: {
            tableRowClassName({row, rowIndex}) {
                if (rowIndex === 1) {
                    return 'warning-row';
                } else if (rowIndex === 3) {
                    return 'success-row';
                }
                return '';
            }
        },
        data() {
            return {
                tableData2: [{
                    number: '21059',
                    length: '26497',
                    total: '184448',
                    date:'2019年'
                }, {
                    number: '18777',
                    length: '21140',
                    total: '188808',
                    date:'2018年'
                },{
                    number: '15757',
                    length: '20225',
                    total: '20114',
                    date:'2017年'
                },{
                    number: '17458',
                    length: '19569',
                    total: '202446',
                    date:'2016年'
                },{
                    number: '11323',
                    length: '14562',
                    total: '279854',
                    date:'2015年'
                }
                ]
            }
        }
    }
</script>

<style lang="scss" scoped>
    .el-table .warning-row {
        background: oldlace;
    }

    .el-table .success-row {
        background: #f0f9eb;
    }
</style>
