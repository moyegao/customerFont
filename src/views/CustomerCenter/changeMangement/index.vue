
<template>
  <div class="CustomerChange">
    <!-- 变更选择框 -->
    <div  v-if="CustomerChangeAddVisible">
      <div class="bread-contain">
        <publicCrumbs :crumbsData="crumbsData"></publicCrumbs>
        <div class="bread-contain-right">
          <!-- <el-button type="primary" size="mini" @click="submit('ruleForm');">保 存</el-button> -->
          <el-button icon="el-icon-caret-left" size="mini" @click="closeDialog('ruleForm')">返回</el-button>
        </div>
      </div>
    </div>
    <div v-else>
      <div class="bread-contain">
        <publicCrumbs :crumbsData="crumbsData"></publicCrumbs>
        <div class="bread-contain-right">
          <el-button type="primary" size="mini" icon="el-icon-plus"  @click="salesAdd">添加</el-button>
        </div>
      </div>
    </div>
    <div  v-if="CustomerChangeAddVisible" style="height: calc(100% - 41px);">
      <CustomerChangeAdd ref="CustomerChangeAdd" :id="id"></CustomerChangeAdd>
    </div>

    <div  v-else>
      <div class="toolbar">
          <el-form :inline="true" size="mini" :model="formData" class="demo-form-inline">

              <el-form-item class="form-left">
              <el-form-item label='单据日期：'>
                  <el-date-picker v-model="DateChoosevalue" type="daterange" value-format="yyyy-MM-dd" range-separator="至" :picker-options="pickerOptions" :clearable="true" start-placeholder="开始日期" end-placeholder="结束日期">
                  </el-date-picker>
              </el-form-item>

              <el-form-item label="状态：">
                  <el-select clearable v-model="formData.watermeterWarehouse" placeholder="未审">
                  <el-option v-for="(item,index) in approvalStatusOptions" :key="index" :label="item.name" :value="item.value"></el-option>

                  </el-select>
              </el-form-item>
              <el-form-item label="模糊查询：" class="searchInput">
                  <el-tooltip class="item" effect="dark" content="单据编号、用户编号、用户名称、受理人" placement="top">
                      <el-input v-model="formData.con" clearable size="mini"></el-input>
                  </el-tooltip>
              </el-form-item>

              <el-form-item>

                  <el-button class='searchBtn' @click="demand" icon="el-icon-search"></el-button>
              </el-form-item>

              </el-form-item>
          </el-form>
      </div>

      <div class="kl-table">
          <el-table v-if="tableShow" :max-height="maxHeight" stripe border :data="histroyData.list" class="change-tables-table">

              <el-table-column type="index" label="NO." width="50" fixed="left" :index="indexMethod">
              </el-table-column>
      
              <el-table-column prop="state" min-width="60" label="状态">
              </el-table-column>

              <el-table-column prop="engineeringState" min-width="100" label="进度">
              </el-table-column>

              <el-table-column prop="handleName" min-width="100" label="当前处理人">
              </el-table-column>

              <!-- <el-table-column prop="useLevel" min-width="100" label="紧急程度" show-overflow-tooltip>
              </el-table-column> -->

              <el-table-column prop="connect3" min-width="100" label="单据编号">
              </el-table-column>

              <el-table-column prop="connect2" min-width="100" label="单据日期">
              </el-table-column>

              <el-table-column prop="connect" min-width="80" label="用户编号">

              </el-table-column>

              <el-table-column prop="lvalue" min-width="80" label="用户名称">
              </el-table-column>

              <el-table-column prop="fixValue" min-width="80" label="变更类型">
              </el-table-column>

              <el-table-column prop="flux" min-width="80" label="变更内容">
              </el-table-column>

              <el-table-column label="操作" fixed="right" width="80">
              <template slot-scope="scope">

                  <el-button type="text" @click="salesAdd(scope.$index, scope.row)">详情</el-button>
              </template>
              </el-table-column>

          </el-table>
          <!-- 分页 -->
          <div class="block">
              <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="1" :page-sizes="[20, 100, 500, 1000]" :page-size="20" layout="total, sizes, prev, pager, next, jumper" :total="histroyData.total">
              </el-pagination>
          </div>
      </div>
    </div>
  </div>
</template>
<script>
import CustomerChangeAdd from './CustomerChangeAdd'
export default {
  name: "CustomerChange",
  components: {
    // CustomerChangeEdit,
    CustomerChangeAdd,
    // chooseuser
  },
  data() {
    return {
      id:'',//详情主键id
      chooseuserName:"选择用户",
      chooseuserVisible:false,
      CustomerChangeAddVisible: false,
      TransferDetailsGH: false,
      historyData: {
        list: [
          {
            month:'201905',
            fname:'污水费',
            value:'12',
            money:'123',
            fare:'正常',
            },
          {
            month:'201805',
            fname:'水费',
            value:'12',
            money:'123',
            fare:'坏账',
            },
          {
            month:'201705',
            fname:'水费',
            value:'12',
            money:'123',
            fare:'呆账',
            },
        ],
      },
      crumbsData: {  //面包屑
        titleList: [
          // { title: '业务管理', path: '/CustomerChange' },
          { title: '客户管理' },
          { title: '变更' },
        ],

      },
      approvalStatusOptions: [
        
        { value: '1', name: '全部' },
        { value: '2', name: '草稿' },
        { value: '3', name: '进行中' },
        { value: '4', name: '已办结' },
        { value: '5', name: '已废弃' },
      ],//审批状态下拉框
      formData: {
        con: "",
        watermeterWarehouse: "1",
        waterMeterManufacturer: "",
        waterMeterManufacturerOptions: [],
        waterMeterType: "",
        waterMeterTypeOptions: [],
        waterMeterCaliber: "",
        waterMeterCaliberOptions: [],
        waterMeterStatus: "",
        waterMeterStatusOptions: [],
        subordinateDepartments: "",
        subordinateDepartmentsOptions: []

      },
      // 日期选择器
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() > Date.now();
        }
      },
      DateChoosevalue: [],
      tableShow: false,
      maxHeight: 0,
      histroyData: {

      },
      CustomerChangeVisible: false,  //综合查询的显示和隐藏
      CustomerChangeName: '销户办理',
      ruleFormData: {},

    }
  },
  mounted() {
    this.init();
    //data 里面定义一个axHeight 用于设置table的高度，增加一个v-if事件用于渲染吗，监听maxHeight 的变化控制v-if的变量

    this.common.changeTable(this, '.water-meter-inquiry .kl-table', ['.water-meter-inquiry .toolbar', '.water-meter-inquiry .block', '.water-meter-inquiry #crumbs'])
  },
  methods: {
    init() {},

    demand() {  //查询
      // this.tableQuery.page = 1
      // this.tableQuery.pageCount = 20
      this.init()
    },
    //   导出
    exportExcel() {
    },
    //分页
    handleSizeChange(val) { //显示多少数据一页
      // this.tableQuery.pageCount = val
      // this.tableQuery.page = 1
      this.init()
    },
    handleCurrentChange(val) {  //显示多少页码
      // this.tableQuery.page = val
      this.init()
    },

    cellClick(row, column, cell, event) { //点击文件名
      let that = this;

    },
    indexMethod(index) {//获取表格序号
      //    return  (this.tableQuery.page-1)*this.tableQuery.pageCount + (index+1) ;
      return (this.histroyData.pageSize - 20) * this.histroyData.pages + (index + 1);

    },
    closeDialog() { //关闭会话
      // this.crumbsData.titleList.pop();
      this.CustomerChangeVisible = false;
      this.backfillVisible = false;
      this.CustomerChangeAddVisible = false;
      this.init()
    },
    closeChooseuser(){
      this.chooseuserVisible = false;
    },
    //弹出框
    submit(formName) {	//提交
      this.$refs.childCustomerChange.submit()
      // this.CustomerChangeVisible = false;
    },
    edit() {
      // this.CustomerChangeVisible = true;
      // this.crumbsData.titleList.push({ title: '编辑', method: () => { window.histroy.back() } })
    },
    salesAdd(){
      this.id = '';
      this.CustomerChangeAddVisible = true;
    //   this.chooseuserVisible = true;
      // this.crumbsData.titleList.push({ title: '添加', method: () => { window.histroy.back() } })
    },
    // 删除行
    deleteRow(index, row) {
      this.$confirm('是否确认删除该行?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.histroyData.list.splice(index, 1)
        this.$message({
          type: 'success',
          message: '删除成功!'
        });
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        });
      });
    }
  },
  watch: {
    maxHeight() {
      this.tableShow = false
      this.$nextTick(() => {
        this.tableShow = true
      })
    },
  }
}
</script>
<style lang="scss">
.CustomerChange {
  width: 100%;
  height: 100%;
}
</style>
