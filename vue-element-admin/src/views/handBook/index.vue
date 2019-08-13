<template>
  <div class="app-container">
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item label="物资编号:">
        <el-select v-model="formInline.name" placeholder="请输入编号">
          <el-option v-for="item in carNameOptions" :key="item" :label="item" :value="item" />
        </el-select>
      </el-form-item>
      <el-form-item label="物资类别:">
        <el-select v-model="formInline.Institute" placeholder="请选择类别">
          <el-option v-for="item in InstituteOptions" :key="item" :label="item" :value="item" />
        </el-select>
      </el-form-item>
      <el-form-item label="物资名称:">
        <el-select v-model="formInline.city" placeholder="请输入物资名称">
          <el-option v-for="item in calendarTypeOptions" :key="item.key" :label="item.display_name" :value="item.key" />
          <!--<el-option label="重庆" value="shanghai"></el-option>-->
          <!--<el-option label="重庆微标科技" value="beijing"></el-option>-->
        </el-select>
      </el-form-item>
      <el-form-item label="规格型号:">
        <el-select v-model="formInline.status" placeholder="请选择型号">
          <el-option v-for="item in statusOptions" :key="item" :autosize="{ minRows: 2, maxRows: 4}" :label="item" :value="item" />
        </el-select>
      </el-form-item>
    </el-form>
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item label="物资批次:">
        <el-select v-model="formInline.static" placeholder="请选择批次">
          <el-option v-for="item in staticOptions" :key="item" :label="item" :value="item" />
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button v-waves class="filter-item" type="primary" icon="el-icon-search" @click="handleFilter">
          {{ $t('table.search') }}
        </el-button>
        <el-button class="filter-item" style="margin-left: 10px;" type="primary" icon="el-icon-edit" @click="handleCreate">
          {{ $t('table.add') }}
        </el-button>
        <el-button v-waves :loading="downloadLoading" class="filter-item" type="primary" icon="el-icon-download" @click="handleDownload">
          {{ $t('table.export') }}
        </el-button>
      </el-form-item>
    </el-form>

    <el-table
      :key="tableKey"
      v-loading="listLoading"
      :data="tableData"
      border
      style="width: 100%;"
      highlight-current-row
      @sort-change="sortChange"
    >
      <el-table-column
        fixed
        prop="MatID"
        label="物资编号"
        width="150"
      />
      <el-table-column
        prop="category"
        label="物资类别"
        min-width="80"
      />
      <el-table-column
        prop="MaterialName"
        label="物资名称"
        min-width="200"
      />
      <el-table-column
        prop="ModelType"
        label="规格型号"
        min-width="150"
      />
      <el-table-column
        prop="Location"
        label="库位信息"
        min-width="150"
      />
      <el-table-column
        prop="prickle"
        label="计量单位"
        width="150"
      />
      <el-table-column
        prop="ExpirationDate"
        label="保质期"
        width="150"
      />
      <el-table-column
        prop="MaterialBatch"
        label="物资批次"
        width="150"
      />
      <el-table-column
        fixed="right"
        label="操作"
        width="230"
      >
        <template slot-scope="scope">
          <el-button type="primary" size="small" @click="handleUpdate(row)">编辑</el-button>
          <el-button type="danger" size="small" @click="handleModifyStatus(row,'deleted')">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <pagination v-show="total>0" :total="total" :page.sync="listQuery.page" :limit.sync="listQuery.limit" @pagination="getList" />

    <el-dialog :title="textMap[dialogStatus]" :visible.sync="dialogFormVisible">
      <el-form :inline="true" :model="formInline" class="demo-form-inline" label-width="90px" style=" margin-left:60px;">
        <el-form-item :label="$t('table.MatID')" prop="name">
          <el-select v-model="temp.MatID" class="filter-item" placeholder="请输入物资编号">
            <el-option v-for="item in carNameOptions" :key="item" :label="item" :value="item" />
          </el-select>
        </el-form-item>
        <el-form-item :label="$t('table.category')" prop="Institute">
          <el-select v-model="temp.category" class="filter-item" placeholder="请输入物资类别">
            <el-option v-for="item in InstituteOptions" :key="item" :label="item" :value="item" />
          </el-select>
        </el-form-item>
      </el-form>
      <el-form :inline="true" :model="formInline" class="demo-form-inline" label-width="90px" style=" margin-left:60px;">
        <el-form-item :label="$t('table.MaterialName')" prop="city">
          <el-select v-model="temp.MaterialName" class="filter-item" placeholder="请输入物资名称">
            <el-option v-for="item in calendarTypeOptions" :key="item.key" :label="item.display_name" :value="item.key" />
          </el-select>
        </el-form-item>
        <el-form-item :label="$t('table.ModelType')">
          <el-select v-model="temp.ModelType" class="filter-item" placeholder="物资型号">
            <el-option v-for="item in statusOptions" :key="item" :autosize="{ minRows: 2, maxRows: 4}" :label="item" :value="item" />
          </el-select>
        </el-form-item>
      </el-form>
      <el-form :inline="true" :model="formInline" class="demo-form-inline" label-width="90px" style="margin-left:60px;">
        <el-form-item :label="$t('table.Location')">
          <el-select v-model="temp.Location" class="filter-item" placeholder="请输入库位信息">
            <el-option v-for="item in staticOptions" :key="item" :label="item" :value="item" />
          </el-select>
        </el-form-item>
        <el-form-item :label="$t('table.prickle')">
          <el-select v-model="temp.prickle" class="filter-item" placeholder="请输入计量单位">
            <el-option v-for="item in prickleOptions" :key="item" :label="item" :value="item" />
          </el-select>
        </el-form-item>
      </el-form>
      <el-form :inline="true" :model="formInline" class="demo-form-inline" label-width="90px" style=" margin-left:60px;">
        <el-form-item :label="$t('table.ExpirationDate')">
          <el-select v-model="temp.ExpirationDate" class="filter-item" placeholder="请输入保质期">
            <el-option v-for="item in ExpirationDate" :key="item" :label="item" :value="item" />
          </el-select>
        </el-form-item>
        <el-form-item :label="$t('table.MaterialBatch')">
          <el-select v-model="temp.MaterialBatch" class="filter-item" placeholder="请输入物资批次">
            <el-option v-for="item in MaterialBatch" :key="item" :label="item" :value="item" />
          </el-select>
        </el-form-item>
      </el-form>
      <!--<el-form ref="dataForm" :rules="rules" :model="temp" label-position="left" label-width="90px" style="width: 400px; margin-left:50px;">-->
      <!---->

      <!---->
      <!---->
      <!---->
      <!--</el-form>-->
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">
          {{ $t('table.cancel') }}
        </el-button>
        <!--<el-button type="primary" @click="dialogStatus==='create'?createData():updateData()">-->
        <!--{{ $t('table.confirm') }}-->
        <!--</el-button>-->
        <el-button type="primary" @click="dialogFormVisible = false">
          {{ $t('table.confirm') }}
        </el-button>
      </div>
    </el-dialog>

    <el-dialog :visible.sync="dialogPvVisible" title="Reading statistics">
      <el-table :data="pvData" border fit highlight-current-row style="width: 100%">
        <el-table-column prop="key" label="Channel" />
        <el-table-column prop="pv" label="Pv" />
      </el-table>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogPvVisible = false">{{ $t('table.confirm') }}</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import { fetchList, fetchPv, createArticle, updateArticle } from '@/api/article'
import waves from '@/directive/waves' // waves directive
import { parseTime } from '@/utils'
import Pagination from '@/components/Pagination' // secondary package based on el-pagination

const calendarTypeOptions = [
  { key: 'CN', display_name: 'RTU箱' },
  { key: 'US', display_name: '通用通信插件' },
  { key: 'JP', display_name: '遥测采集模块' },
  { key: 'EU', display_name: '电池巡检单元' }
]

// arr to obj, such as { CN : "China", US : "USA" }
const calendarTypeKeyValue = calendarTypeOptions.reduce((acc, cur) => {
  acc[cur.key] = cur.display_name
  return acc
}, {})

export default {
  name: 'ComplexTable',
  components: { Pagination },
  directives: { waves },
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'info',
        deleted: 'danger'
      }
      return statusMap[status]
    },
    typeFilter(type) {
      return calendarTypeKeyValue[type]
    }
  },
  data() {
    return {
      tableKey: 0,
      total: 0,
      listLoading: true,
      listQuery: {
        page: 1,
        limit: 10,
        importance: undefined,
        title: undefined,
        type: undefined,
        sort: '+id'
      },
      formInline: {
        name: '',
        Institute: '',
        city: '',
        status: '',
        static: ''
      },
      // table数据
      tableData: [{
        MatID: '30000001000',
        category: '主要材料',
        MaterialName: 'RTU箱',
        ModelType: 'DYW- TDT3200',
        Location: '模块化装备储备库',
        prickle: '个',
        ExpirationDate: '4年',
        MaterialBatch: '生产批次'
      }, {
        MatID: '30000001001',
        category: '配件',
        MaterialName: '通用通信插件',
        ModelType: 'DYW- 892',
        Location: '器材装备储备库',
        prickle: '块',
        ExpirationDate: '3年',
        MaterialBatch: '管理批次'
      }, {
        MatID: '30000001002',
        category: '辅助材料',
        MaterialName: '遥测采集模块',
        ModelType: 'DYW- 7-A/AI16-15AIX 01DI',
        Location: '被装物资储备库',
        prickle: '块',
        ExpirationDate: '3年',
        MaterialBatch: '生产批次'
      }, {
        MatID: '30000002000',
        category: '动力',
        MaterialName: '开关电源模块',
        ModelType: 'NK- AC220V',
        Location: '被装物资储备库',
        prickle: '个',
        ExpirationDate: '4年',
        MaterialBatch: '生产批次'
      }, {
        MatID: '30000002001',
        category: '燃料',
        MaterialName: '电池巡检单元',
        ModelType: 'NK- PM2B.DC110V',
        Location: '被装物资储备库',
        prickle: '个',
        ExpirationDate: '2年',
        MaterialBatch: '管理批次'
      }, {
        MatID: '30000002002',
        category: '配件',
        MaterialName: '绝缘监控单元',
        ModelType: 'NK- PM2I.DC110V',
        Location: '模块化装备储备库',
        prickle: '个',
        ExpirationDate: '3年',
        MaterialBatch: '管理批次'
      }, {
        MatID: '300000020008',
        category: '主要材料',
        MaterialName: '接触网开关监控单元',
        ModelType: 'NK- 5730',
        Location: '器材装备储备库',
        prickle: '套',
        ExpirationDate: '3年',
        MaterialBatch: '管理批次'
      }, {
        MatID: '300000020100',
        category: '工具',
        MaterialName: 'IATP-102远方控制装置',
        ModelType: 'NK-102',
        Location: '器材装备储备库',
        prickle: '台',
        ExpirationDate: '5年',
        MaterialBatch: '管理批次'
      }, {
        MatID: '300000022000',
        category: '主要材料',
        MaterialName: '电力远动光电交换机',
        ModelType: 'NK-2000',
        Location: '模块化装备储备库',
        prickle: '块',
        ExpirationDate: '2年',
        MaterialBatch: '管理批次'
      }, {
        MatID: '301800000206',
        category: '配件',
        MaterialName: '开关监控装置',
        ModelType: 'DSL-31DC',
        Location: '器材装备储备库',
        prickle: '个',
        ExpirationDate: '2年',
        MaterialBatch: '管理批次'
      }
      ],
      calendarTypeOptions,
      sortOptions: [{ label: 'ID Ascending', key: '+id' }, { label: 'ID Descending', key: '-id' }],
      carNameOptions: ['300000022000', '30000001001', '301800000206'],
      InstituteOptions: ['主要材料', '配件', '工具', '辅助材料'],
      statusOptions: ['DYW- TDT3200', 'DSL-31DC', 'NK-2000', 'NK- PM2B.DC110V'],
      staticOptions: ['模块化装备储备库', '器材装备储备库', '被装物资储备库'],
      prickleOptions: ['个', '块', '套', '台'],
      ExpirationDate: ['2年', '3年', '4年', '5年'],
      MaterialBatch: ['生产批次', '管理批次'],
      showReviewer: false,
      temp: {
        name: 'mini消防车',
        Institute: '',
        static: '正常',
        status: 'DYW- TDT3200'
      },
      dialogFormVisible: false,
      dialogStatus: '',
      textMap: {
        update: '编辑物资',
        create: '添加物资'
      },
      dialogPvVisible: false,
      pvData: [],
      rules: {
      },
      downloadLoading: false
    }
  },
  created() {
    this.getList()
  },
  methods: {
    handleClick(row) {
      console.log(row)
    },
    getList() {
      this.listLoading = true
      fetchList(this.listQuery).then(response => {
        this.list = response.data.items
        this.total = response.data.total

        // Just to simulate the time of the request
        setTimeout(() => {
          this.listLoading = false
        }, 1.5 * 100)
      })
    },
    handleFilter() {
      this.listQuery.page = 1
      this.getList()
    },
    handleModifyStatus(row, status) {
      this.$message({
        message: '操作成功',
        type: 'success'
      })
      row.status = status
    },
    sortChange(data) {
      const { prop, order } = data
      if (prop === 'id') {
        this.sortByID(order)
      }
    },
    sortByID(order) {
      if (order === 'ascending') {
        this.listQuery.sort = '+id'
      } else {
        this.listQuery.sort = '-id'
      }
      this.handleFilter()
    },
    resetTemp() {
      this.temp = {
        id: undefined,
        importance: 1,
        remark: '',
        timestamp: new Date(),
        title: '',
        status: 'DYW- TDT3200',
        type: ''
      }
    },
    handleCreate() {
      this.resetTemp()
      this.dialogStatus = 'create'
      this.dialogFormVisible = true
      this.$nextTick(() => {
        this.$refs['dataForm'].clearValidate()
      })
    },
    createData() {
      this.$refs['dataForm'].validate((valid) => {
        if (valid) {
          this.temp.id = parseInt(Math.random() * 100) + 1024 // mock a id
          this.temp.author = 'vue-element-goods'
          createArticle(this.temp).then(() => {
            this.list.unshift(this.temp)
            this.dialogFormVisible = false
            this.$notify({
              title: '成功',
              message: '创建成功',
              type: 'success',
              duration: 2000
            })
          })
        }
      })
    },
    handleUpdate(row) {
      this.temp = Object.assign({}, row) // copy obj
      // this.temp.timestamp = new Date(this.temp.timestamp)
      this.dialogStatus = 'update'
      this.dialogFormVisible = true
      this.$nextTick(() => {
        this.$refs['dataForm'].clearValidate()
      })
    },
    updateData() {
      this.$refs['dataForm'].validate((valid) => {
        if (valid) {
          const tempData = Object.assign({}, this.temp)
          // tempData.timestamp = +new Date(tempData.timestamp) // change Thu Nov 30 2017 16:41:05 GMT+0800 (CST) to 1512031311464
          updateArticle(tempData).then(() => {
            for (const v of this.list) {
              if (v.id === this.temp.id) {
                const index = this.list.indexOf(v)
                this.list.splice(index, 1, this.temp)
                break
              }
            }
            this.dialogFormVisible = false
            this.$notify({
              title: '成功',
              message: '更新成功',
              type: 'success',
              duration: 2000
            })
          })
        }
      })
    },
    handleDelete(row) {
      this.$notify({
        title: '成功',
        message: '删除成功',
        type: 'success',
        duration: 2000
      })
      const index = this.list.indexOf(row)
      this.list.splice(index, 1)
    },
    handleFetchPv(pv) {
      fetchPv(pv).then(response => {
        this.pvData = response.data.pvData
        this.dialogPvVisible = true
      })
    },
    handleDownload() {
      this.downloadLoading = true
        import('@/vendor/Export2Excel').then(excel => {
          const tHeader = ['timestamp', 'title', 'type', 'importance', 'status']
          const filterVal = ['timestamp', 'title', 'type', 'importance', 'status']
          const data = this.formatJson(filterVal, this.list)
          excel.export_json_to_excel({
            header: tHeader,
            data,
            filename: 'table-list'
          })
          this.downloadLoading = false
        })
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => {
        if (j === 'timestamp') {
          return parseTime(v[j])
        } else {
          return v[j]
        }
      }))
    }
  }
}
</script>
<style>
    .el-dialog {
        position: relative;
        margin: 0 auto 50px;
        background: #FFFFFF;
        border-radius: 2px;
        -webkit-box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
        box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
        width: 40%;
        margin-top: 25vh !important;
    }
</style>
