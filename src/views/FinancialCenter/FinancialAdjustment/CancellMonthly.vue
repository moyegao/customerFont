

<template>

  <div class="CancellMonthly">
    <div class="bread-contain">
      <publicCrumbs :crumbsData="crumbsData"></publicCrumbs>
      <div class="bread-contain-right" v-if="CancellMonthlyEditVisible">
        <el-button icon="el-icon-caret-left" size="mini" @click="closeDialog">返回</el-button>
      </div>
      <div class="bread-contain-right" v-else>
        <el-button size="mini" @click="add" type="primary" icon="el-icon-plus">添加</el-button>
      </div>
    </div>

    <CancellMonthlyEdit v-if="CancellMonthlyEditVisible" ref="childRedFlushing"></CancellMonthlyEdit>

    <div v-else>

      <div class="toolbar">
        <el-form :inline="true" size="mini" :model="formData" class="demo-form-inline">

          <el-form-item class="form-left">
            <el-form-item label="状态：">
              <el-select clearable v-model="formData.waterMeterCaliber" placeholder="未审">
                <el-option v-for="(item,index) in approvalStatusOptions" :key="index" :label="item.name" :value="item.value"></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="用户名称：">
              <el-select clearable v-model="formData.watermeterWarehouse" placeholder="管理员">
                <el-option v-for="(item,index) in formData.watermeterWarehouseOptions" :key="index" :label="item.name" :value="item.value"></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="模糊查询：" class="width-200">
              <el-input v-model="formData.con" placeholder="用户编号/用户名称/账期"></el-input>
            </el-form-item>

            <el-form-item>
              <el-button class='searchBtn' @click="demand" icon="el-icon-search"></el-button>
            </el-form-item>

          </el-form-item>
        </el-form>
      </div>
      <div class="kl-table">
        <el-table v-if="tableShow" :max-height="maxHeight" stripe border :data="histroyData.list" class="change-tables-table">

          <el-table-column type="index" label="NO." width="50" :index="indexMethod">
          </el-table-column>

           

          <el-table-column prop="state" min-width="60" label="状态">
          </el-table-column>

          <el-table-column prop="engineeringState" min-width="100" label="进度">
          </el-table-column>

          <el-table-column prop="handleName" min-width="100" label="当前处理人">
          </el-table-column>

          <el-table-column prop="concentrator1" min-width="100" label="单据编号">
          </el-table-column>

          <el-table-column prop="concentrator1" min-width="100" label="单据日期">
          </el-table-column>

          <el-table-column prop="code" min-width="80" label="用户编号" show-overflow-tooltip>
          </el-table-column>

          <el-table-column prop="lvalue" min-width="100" label="用户名称">
          </el-table-column>

          <el-table-column prop="lvalue" min-width="100" label="账期">
          </el-table-column>

          <el-table-column prop="flux" min-width="80" label="票据号码">
          </el-table-column>

          <el-table-column label="操作" fixed="right" width="80">
            <template slot-scope="scope">
              <el-button type="text" @click="add(scope.$index, scope.row)">详情</el-button>
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
import CancellMonthlyEdit from "./CancellMonthlyEdit";
export default {
  name: "CancellMonthly",
  components: {
    CancellMonthlyEdit
  },
  data() {
    return {
      CancellMonthlyEditVisible: false,
      crumbsData: {
        //面包屑
        titleList: [
          { title: "收费管理" },
          { title: "账务调整" },
          { title: "取消当月收费" }
        ]
      },
      formData: {
        con: "",
        watermeterWarehouse: "",
        watermeterWarehouseOptions: [
          {
            name: "徐林",
            value: "1"
          },
          {
            name: "系统管理员",
            value: "2"
          },
          {
            name: "王敬农",
            value: "3"
          },
          {
            name: "陈秀梅",
            value: "4"
          }
        ],
        waterMeterManufacturer: "",
        waterMeterManufacturerOptions: [],
        waterMeterType: "",
        waterMeterTypeOptions: [],
        waterMeterCaliber: "1",
        waterMeterStatus: "",
        waterMeterStatusOptions: [],
        subordinateDepartments: "",
        subordinateDepartmentsOptions: []
      },
      //审批状态下拉框
      approvalStatusOptions: [
        { value: "1", name: "全部" },
        { value: "2", name: "草稿" },
        { value: "3", name: "进行中" },
        { value: "4", name: "已办结" },
        { value: "5", name: "已废弃" }
      ],
      // 日期选择器
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() > Date.now();
        }
      },
      DateChoosevalue: [],
      tableShow: false,
      maxHeight: 0,
      histroyData: {},
      CancellMonthlyVisible: false, //综合查询的显示和隐藏
      CancellMonthlyName: "坏账处理-增加",
      ruleFormData: {},
      multipleSelection: []
    };
  },
  mounted() {
    this.init();
    //data 里面定义一个axHeight 用于设置table的高度，增加一个v-if事件用于渲染吗，监听maxHeight 的变化控制v-if的变量

    this.common.changeTable(this, ".CancellMonthly .kl-table", [
      ".CancellMonthly .toolbar",
      ".CancellMonthly .block",
      ".CancellMonthly #crumbs"
    ]);
  },
  methods: {
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },
    init() {
      this.histroyData = {
        areaId: "",
        endRow: 5,
        hasNextPage: false,
        hasPreviousPage: false,
        isFirstPage: true,
        isLastPage: true,
        list: [
          {
            areaId: null,
            areaName: "东深小区",
            code: "C7551990000003",
            concentrator: "",
            connect: null,
            daytime: "2019-07-02",
            endTime: null,
            factoryName: "东井",
            fcode: null,
            fix: false,
            fixValue: "-",
            flux: "-",
            getMeter: 1,
            id: null,
            isxz: "未抄",
            lvalue: "-",
            meter: null,
            model: null,
            modifyBy: null,
            modifyTime: null,
            offset: 0,
            page: 1,
            pageCount: 20,
            position: "1栋3单元",
            protocolName: null,
            rowNumber: 0,
            sensorModel: null,
            sn: null,
            status: "未上传",
            totalPage: 0,
            type: 5,
            unusual: null,
            usn: "00",
            waterGroup: null
          },
          {
            areaId: null,
            areaName: "东深小区",
            code: "C7551990000003",
            concentrator: "",
            connect: null,
            daytime: "2019-07-02",
            endTime: null,
            factoryName: "东井",
            fcode: null,
            fix: false,
            fixValue: "-",
            flux: "-",
            getMeter: 1,
            id: null,
            isxz: "未抄",
            lvalue: "-",
            meter: null,
            model: null,
            modifyBy: null,
            modifyTime: null,
            offset: 0,
            page: 1,
            pageCount: 20,
            position: "1栋3单元",
            protocolName: null,
            rowNumber: 0,
            sensorModel: null,
            sn: null,
            status: "未上传",
            totalPage: 0,
            type: 5,
            unusual: null,
            usn: "00",
            waterGroup: null
          },
          {
            areaId: null,
            areaName: "东深小区",
            code: "C7551990000003",
            concentrator: "",
            connect: null,
            daytime: "2019-07-02",
            endTime: null,
            factoryName: "东井",
            fcode: null,
            fix: false,
            fixValue: "-",
            flux: "-",
            getMeter: 1,
            id: null,
            isxz: "未抄",
            lvalue: "-",
            meter: null,
            model: null,
            modifyBy: null,
            modifyTime: null,
            offset: 0,
            page: 1,
            pageCount: 20,
            position: "1栋3单元",
            protocolName: null,
            rowNumber: 0,
            sensorModel: null,
            sn: null,
            status: "未上传",
            totalPage: 0,
            type: 5,
            unusual: null,
            usn: "00",
            waterGroup: null
          },
          {
            areaId: null,
            areaName: "东深小区",
            code: "C7551990000003",
            concentrator: "",
            connect: null,
            daytime: "2019-07-02",
            endTime: null,
            factoryName: "东井",
            fcode: null,
            fix: false,
            fixValue: "-",
            flux: "-",
            getMeter: 1,
            id: null,
            isxz: "未抄",
            lvalue: "-",
            meter: null,
            model: null,
            modifyBy: null,
            modifyTime: null,
            offset: 0,
            page: 1,
            pageCount: 20,
            position: "1栋3单元",
            protocolName: null,
            rowNumber: 0,
            sensorModel: null,
            sn: null,
            status: "未上传",
            totalPage: 0,
            type: 5,
            unusual: null,
            usn: "00",
            waterGroup: null
          }
        ],
        navigateFirstPage: 1,
        navigateLastPage: 1,
        navigatePages: 8,
        navigatepageNums: Array(1),
        nextPage: 0,
        pageNum: 1,
        pageSize: 20,
        pages: 1,
        prePage: 0,
        size: 5,
        startRow: 1,
        total: 5
      };
    },

    demand() {
      //查询
      // this.tableQuery.page = 1
      // this.tableQuery.pageCount = 20
      this.init();
    },
    //   导出
    exportExcel() {},
    //分页
    handleSizeChange(val) {
      //显示多少数据一页
      // this.tableQuery.pageCount = val
      // this.tableQuery.page = 1
      this.init();
    },
    handleCurrentChange(val) {
      //显示多少页码
      // this.tableQuery.page = val
      this.init();
    },

    cellClick(row, column, cell, event) {
      //点击文件名
      let that = this;
    },
    indexMethod(index) {
      //获取表格序号
      //    return  (this.tableQuery.page-1)*this.tableQuery.pageCount + (index+1) ;
      return (
        (this.histroyData.pageSize - 20) * this.histroyData.pages + (index + 1)
      );
    },
    closeDialog() {
      //关闭会话
      this.CancellMonthlyEditVisible = false;
      this.backfillVisible = false;
      this.init();
    },
    //弹出框
    submit(formName) {
      //提交
      this.$refs.childCancellMonthly.submit();
      // this.CancellMonthlyVisible = false;
    },
    add() {
      this.CancellMonthlyEditVisible = true;
    },
    edit() {}
  },
  watch: {
    maxHeight() {
      this.tableShow = false;
      this.$nextTick(() => {
        this.tableShow = true;
      });
    }
  }
};
</script>
<style lang="scss">
.CancellMonthly {
  width: 100%;
  height: 100%;
}
</style>
