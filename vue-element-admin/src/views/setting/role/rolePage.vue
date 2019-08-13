<template>
    <div class="app-container">
        <el-form :inline="true" :model="formInline" class="demo-form-inline">
            <el-form-item>
                <el-input v-model="input1" placeholder="搜索关键字" >
                    <template slot="prepend">角色名</template>
                </el-input>
            </el-form-item>
            <el-form-item>
                <el-input v-model="input2" placeholder="yyyy-MM-dd HH:mm:ss" >
                    <template slot="prepend">创建时间</template>
                </el-input>
            </el-form-item>
            <el-form-item>
                <el-button v-waves class="filter-item" type="primary" icon="el-icon-search" @click="handleFilter">
                    {{ $t('table.search') }}
                </el-button>
                <el-button class="filter-item" style="margin-left: 10px;" type="primary" icon="el-icon-edit" @click="handleCreate">
                    {{ $t('table.add') }}
                </el-button>
                <el-button v-waves :loading="downloadLoading" class="filter-item" type="danger"  @click="handleDownload">
                    {{ $t('table.chooseDelete') }}
                </el-button>
            </el-form-item>
        </el-form>
        <el-table
            :data="tableData"
            border
            style="width: 100%"
            @sort-change="sortChange"
            :key="tableKey"
            highlight-current-row
            v-loading="listLoading"
        >
            <el-table-column
                prop="id"
                label=""
                width="55">
            </el-table-column>
            <el-table-column
                type="selection"
                width="55">
            </el-table-column>
            <el-table-column
                prop="loginName"
                label="登录名"
                min-width="150px">
            </el-table-column>
            <el-table-column
                prop="name"
                label="姓名"
                min-width="150px">
            </el-table-column>
            <el-table-column
                prop="email"
                label="邮箱"
                min-width="150px">
            </el-table-column>
            <el-table-column
                prop="system"
                label="系统UI"
                min-width="200px">
            </el-table-column>
            <el-table-column
                prop="loginTime"
                label="最后登录时间"
                sortable
                min-width="200px">
            </el-table-column>
            <el-table-column
                prop="createDate"
                label="创建时间"
                sortable
                min-width="200px">
            </el-table-column>
            <el-table-column
                fixed="right"
                label="操作"
                width="230">
                <template slot-scope="scope">
                    <el-button @click="handleUpdate(row)" type="primary" size="small" >编辑</el-button>
                    <el-button @click="handleModifyStatus(row,'deleted')" type="danger" size="small">删除</el-button>
                </template>
            </el-table-column>
        </el-table>

        <div class="block">

            <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="currentPage4"
                :page-sizes="[100, 200, 300, 400]"
                :page-size="100"
                layout="total, sizes, prev, pager, next, jumper"
                :total="400">
            </el-pagination>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                input1: '',
                input2: '',
                currentPage4: 10,
                // table数据
                tableData: [{
                    id: '1',
                    loginName: 'admin',
                    name: '管理员',
                    email: 'test@163.com',
                    system: 'skin2',
                    loginTime:'2019-7-30 9:12:56',
                    createTime:'2018-7-30 9:12:56'
                }, {
                    id: '2',
                    loginName: 'wl',
                    name: '王璐',
                    email: 'test@163.com',
                    system: 'skin2',
                    loginTime:'2019-7-30 9:12:56',
                    createTime:'2018-7-30 9:12:56'
                }, {
                    id: '3',
                    loginName: 'hzm',
                    name: '贺贞梅',
                    email: 'test@163.com',
                    system: 'skin2',
                    loginTime:'2019-7-30 9:12:56',
                    createTime:'2018-7-30 9:12:56'
                },{
                    id: '4',
                    loginName: 'admin',
                    name: '管理员',
                    email: 'test@163.com',
                    system: 'skin2',
                    loginTime:'2019-7-30 9:12:56',
                    createTime:'2018-7-30 9:12:56'
                },{
                    id: '5',
                    loginName: 'lkl',
                    name: '李康林',
                    email: 'test@163.com',
                    system: 'skin2',
                    loginTime:'2019-7-30 9:12:56',
                    createTime:'2018-7-30 9:12:56'
                },{
                    id: '6',
                    loginName: 'srs',
                    name: '时荣升',
                    email: 'test@163.com',
                    system: 'skin2',
                    loginTime:'2019-7-30 9:12:56',
                    createTime:'2018-7-30 9:12:56'
                },{
                    id: '7',
                    loginName: 'wdl',
                    name: '王德练',
                    email: 'test@163.com',
                    system: 'skin2',
                    loginTime:'2019-7-30 9:12:56',
                    createTime:'2018-7-30 9:12:56'
                },{
                    id: '8',
                    loginName: 'lmy',
                    name: '李梦瑶',
                    email: 'test@163.com',
                    system: 'skin2',
                    loginTime:'2019-7-30 9:12:56',
                    createTime:'2018-7-30 9:12:56'
                },{
                    id: '9',
                    loginName: 'zbf',
                    name: '周帮福',
                    email: 'test@163.com',
                    system: 'skin2',
                    loginTime:'2019-7-30 9:12:56',
                    createTime:'2018-7-30 9:12:56'
                },{
                    id: '10',
                    loginName: 'admin',
                    name: '管理员',
                    email: 'test@163.com',
                    system: 'skin2',
                    loginTime:'2019-7-30 9:12:56',
                    createTime:'2018-7-30 9:12:56'
                }
                ],
            }
        },
        methods: {
            handleSizeChange(val) {
                // console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                // console.log(`当前页: ${val}`);
            }
        },
    }
</script>

<style lang="scss">
    .block{
        margin-top: 100px;
    }
</style>
