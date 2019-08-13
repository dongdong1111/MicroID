<template>
    <el-table
        :data="tableData2"
        style="width: 400px;"
        :row-class-name="tableRowClassName">
        <el-table-column
            prop="provinceIn"
            label="省内(件)"
            width="80">
        </el-table-column>
        <el-table-column
            prop="out"
            label="省外(件)"
            width="80">
        </el-table-column>
        <el-table-column
            prop="international"
            label="国际(件)"
            width="80">
        </el-table-column>
        <el-table-column
            prop="business"
            label="总业务量"
            width="80">
        </el-table-column>
        <el-table-column
            prop="date"
            label="日期"
            width="70">
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
                    provinceIn: '21352',
                    out: '26497',
                    international: '184448',
                    business:'2222',
                    date:'2018年'
                }, {
                    provinceIn: '21059',
                    out: '26497',
                    international: '184448',
                    business:'22222',
                    date:'2017年'
                },{
                    provinceIn: '21059',
                    out: '26497',
                    international: '184448',
                    business:'22222',
                    date:'2016年'
                },{
                    provinceIn: '21059',
                    out: '26497',
                    international: '184448',
                    business:'22222',
                    date:'2015年'
                },{
                    provinceIn: '21059',
                    out: '26497',
                    international: '184448',
                    business:'22222',
                    date:'2014年'
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
