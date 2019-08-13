<template>
  <div class="app-container">
    <div class="filter-container">
      <el-input v-model="listQuery.title" :placeholder="$t('table.code')" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-select v-model="listQuery.type" :placeholder="$t('table.name')" clearable class="filter-item" style="width: 130px">
        <el-option v-for="item in calendarTypeOptions" :key="item.key" :label="item.display_name" :value="item.key" />
      </el-select>
      <el-button v-waves class="filter-item" type="primary" icon="el-icon-search" @click="handleFilter">
        {{ $t('table.search') }}
      </el-button>
      <el-button class="filter-item" style="margin-left: 10px;" type="primary" icon="el-icon-edit" @click="handleCreate">
        {{ $t('table.add') }}
      </el-button>
      <el-button v-waves :loading="downloadLoading" class="filter-item" type="primary" icon="el-icon-download" @click="handleDownload">
        {{ $t('table.export') }}
      </el-button>
    </div>
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
        fixed
        prop="id"
        label="序号"
        width="80"
      >
      </el-table-column>
      <el-table-column
        prop="code"
        label="供应商编码"
        min-width="150px">
      </el-table-column>
      <el-table-column
        prop="name"
        label="供应商名称"
        min-width="150px">
      </el-table-column>
      <el-table-column
        prop="city"
        label="所属城市"
        min-width="150px">
      </el-table-column>
      <el-table-column
        prop="number"
        label="供应商电话"
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
    <pagination v-show="total>0" :total="total" :page.sync="listQuery.page" :limit.sync="listQuery.limit" @pagination="getList" />
    <el-dialog :title="textMap[dialogStatus]" :visible.sync="dialogFormVisible">
      <el-form :inline="true" :model="formInline" class="demo-form-inline" label-width="90px" style=" margin-left:60px;">
        <el-form-item :label="$t('table.code')" prop="code" >
          <el-select v-model="temp.code" class="filter-item" placeholder="请输入供应商编码">
            <el-option v-for="item in codeOptions" :key="item" :label="item" :value="item" />
          </el-select>
        </el-form-item>
        <el-form-item :label="$t('table.name')" prop="name" >
          <el-select v-model="temp.name" class="filter-item" placeholder="请输入供应商名称">
            <el-option v-for="item in codeNames" :key="item" :label="item" :value="item" />
          </el-select>
        </el-form-item>
      </el-form>
        <el-form :inline="true" :model="formInline" class="demo-form-inline" label-width="90px" style=" margin-left:60px;">
        <el-form-item :label="$t('table.city')" prop="city">
          <el-select v-model="temp.city" class="filter-item" placeholder="请输入所在城市">
            <el-option v-for="item in calendarTypeOptions" :key="item.key" :label="item.display_name" :value="item.key" />
          </el-select>
        </el-form-item>
        <el-form-item :label="$t('table.mobile')">
          <el-select v-model="temp.mobile" class="filter-item" placeholder="请输入电话">
            <el-option v-for="item in mobileOptions" :key="item" :label="item" :value="item" />
          </el-select>
        </el-form-item>
      </el-form>
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
  { key: 'CN', display_name: '消防车' },
  { key: 'US', display_name: '消防车配件' },
  { key: 'JP', display_name: '重庆消防科技有限公司' },
  { key: 'EU', display_name: '小型消防车' }
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
      list: null,
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

      // table数据
      tableData: [{
        id: '1',
        code: '2019072601',
        name: '消防车',
        city: '渝北区',
        number: '13025803690'
      }, {
        id: '2',
        code: '2019072602',
        name: '消防车配件',
        city: '渝北区',
        number: '13025803690'
      }, {
        id: '3',
        code: '2019072603',
        name: '消防车',
        city: '渝北区',
        number: '13025803690'
      }, {
        id: '4',
        code: '2019072604',
        name: '消防车配件',
        city: '渝北区',
        number: '13025803690'
      }, {
        id: '5',
        code: '2019072605',
        name: '消防车',
        city: '渝北区',
        number: '13025803690'
      }, {
        id: '6',
        code: '2019072606',
        name: '消防车配件',
        city: '渝北区',
        number: '13025803690'
      },{
        id: '7',
        code: '2019072607',
        name: '重庆消防科技有限公司',
        city: '渝北区',
        number: '13025803690'
      },{
        id: '8',
        code: '2019072608',
        name: '重庆消防科技有限公司',
        city: '渝北区',
        number: '13025803690'
      },{
        id: '9',
        code: '2019072609',
        name: '贵州消防科技有限公司',
        city: '渝北区',
        number: '13025803690'
      }, {
        id: '10',
        code: '2019072610',
          name: '重庆',
        city: '渝北区',
        number: '13025803690'
      }
      ],
      codeOptions:[2019072601,2019072602,2019072603,2019072604],
      codeNames:['重庆消防科技有限公司','贵州消防科技有限公司','消防车'],
      mobileOptions:[13025803069,13125803690],
      calendarTypeOptions,
      sortOptions: [{ label: 'ID Ascending', key: '+id' }, { label: 'ID Descending', key: '-id' }],
      statusOptions: ['published', 'draft', 'deleted'],
      showReviewer: false,
      temp: {
        id: undefined,
        code:'',
        city:'',
        mobile:'',
      },
      dialogFormVisible: false,
      dialogStatus: '',
      textMap: {
        update: '编辑供应商',
        create: '添加供应商'
      },
      dialogPvVisible: false,
      pvData: [],
      rules: {
        type: [{ required: true, message: 'type is required', trigger: 'change' }],
        //timestamp: [{ type: 'date', required: true, message: 'timestamp is required', trigger: 'change' }],
        title: [{ required: true, message: 'title is required', trigger: 'blur' }]
      },
      downloadLoading: false
    }
  },
  created() {
    this.getList()
  },
  methods: {
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
        status: 'published',
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
      //this.temp.timestamp = new Date(this.temp.timestamp)
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
          //tempData.timestamp = +new Date(tempData.timestamp) // change Thu Nov 30 2017 16:41:05 GMT+0800 (CST) to 1512031311464
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
