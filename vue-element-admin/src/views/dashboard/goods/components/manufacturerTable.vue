<template>
    <div style="height: 100%">
        <div class="title">
            <h5>各厂家消防车占比</h5>
        </div>
        <div class="content">
            <el-table
                :data="tableData">
                <el-table-column
                    prop="field1"
                    label="排名">
                </el-table-column>
                <el-table-column
                    prop="field2"
                    label="生产厂家">
                </el-table-column>
                <el-table-column
                    prop="field3"
                    label="数值">
                </el-table-column>
                <el-table-column
                    prop="field4"
                    label="占比">
                </el-table-column>
            </el-table>
        </div>
    </div>
</template>

<script>
    import { Table, TableColumn } from 'element-ui'

    export default {
        data() {
            return {
                tableData: [{
                    field1: '1',
                    field2: '重庆TT',
                    field3: '36',
                    field4: '30%'
                }, {
                    field1: '2',
                    field2: '北京YY',
                    field3: '26',
                    field4: '34%'
                }, {
                    field1: '3',
                    field2: '天津TT',
                    field3: '36',
                    field4: '10%'
                }, {
                    field1: '4',
                    field2: '上海TT',
                    field3: '26',
                    field4: '4%'
                }, {
                    field1: '5',
                    field2: '上海TT',
                    field3: '26',
                    field4: '4%'
                }]
            }
        },
        components: {
            elTable: Table,
            elTableColumn: TableColumn
        }
    }
</script>

<style lang="scss" scoped>
    /*浏览器滚动条*/
    ::-webkit-scrollbar {
        width: 10px;
        height: 10px
    }

    ::-webkit-scrollbar-button:vertical {
        display: none
    }

    ::-webkit-scrollbar-corner, ::-webkit-scrollbar-track {
        background-color: #e2e2e2
    }

    ::-webkit-scrollbar-thumb {
        border-radius: 2px;
        background-color: rgba(0, 0, 0, .3)
    }

    ::-webkit-scrollbar-thumb:vertical:hover {
        background-color: rgba(0, 0, 0, .35)
    }

    ::-webkit-scrollbar-thumb:vertical:active {
        background-color: rgba(0, 0, 0, .38)
    }

    .title {
        margin-bottom: 10px;
        padding: 5px 0 10px;
        border-bottom: 1px solid #ebeef5;
        color: #737880;

        h5 {
            margin: 0;
        }
    }

    .content {
        height: calc(100% - 42px);
        overflow: auto;
    }
</style>
