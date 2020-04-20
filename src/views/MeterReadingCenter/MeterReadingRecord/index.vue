<template>
  <div class="Meter-Reading-Record">
    <div class="bread-contain">
      <publicCrumbs :crumbsData="crumbsData"></publicCrumbs>
      <!-- //初始化按钮 -->
      <div class="bread-contain-right" v-if='divChange==1'>
        <el-button size="mini" type="primary" @click="luru()">录入</el-button>
        <el-button size="mini" type="primary" @click="jichao()" icon="el-icon-download">集抄导入</el-button>
        <el-button size="mini" type="primary" icon="el-icon-bottom">下载模板</el-button>
        <el-button size="mini" type="primary" icon="el-icon-download">导入</el-button>
        <el-button size="mini" type="primary" icon="el-icon-upload2" @click="exportExcel()">导出</el-button>
      </div>
      <!-- //录入 -->
      <div class="bread-contain-right" v-else-if='divChange==0'>
        <el-button icon="el-icon-caret-left" size="mini" @click="callBack()">返回</el-button>
      </div>
      <!-- //集抄导入 -->
      <div class="bread-contain-right" v-else-if='divChange==2'>

        <el-button size="mini" type="primary">删除</el-button>
        <el-button size="mini" type="primary">保存</el-button>
        <el-button icon="el-icon-caret-left" size="mini" @click="callBack()">返回</el-button>
      </div>
    </div>
    <div class="company-content" v-if='divChange==1'>
      <div class="company-left">
        <company-tree :treeData="treeDatas" @sendTreeData="backTreeData"></company-tree>
      </div>
      <div class="company-right">
        <el-form :inline="true" size="mini" :model="formData" class="demo-form-inline" style="padding: 0px 20px;">
          <el-form-item label="册本：">01354S</el-form-item>
          <el-form-item label="总水量：">
            <font color="red">0</font>
          </el-form-item>
          <el-form-item label="总户数：">
            <font color="red">44</font>
          </el-form-item>
          <el-form-item label="异常户数：">
            <font color="red">04</font>
          </el-form-item>
          <el-form-item label="应抄：">
            <font>44</font>
          </el-form-item>
          <el-form-item label="已抄：">
            <font color="red">04</font>
          </el-form-item><br>
          <el-form-item label="账期：">
            <el-date-picker v-model="formData.beginYearM" size="mini" type="month" unlink-panels format="yyyyMM" value-format="yyyyMM" placeholder="选择月" style="width:100%;">
            </el-date-picker>
          </el-form-item>
          <el-form-item label="用户编号：" class="longinput">
            <el-input v-model="formData.inputContent" clearable placeholder="用户编号"></el-input>
          </el-form-item>
          <el-form-item label="抄表状态：" class="longinput">
            <el-select style="width: 100%" v-model="formData.TableStatus" clearable size="mini">
              <el-option v-for="item in TableStatusList" :key="item.value" :label="item.label" :value="item.value">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item>
            <el-button class="searchBtn" @click="search" icon="el-icon-search"></el-button>
          </el-form-item>
        </el-form>
        <div class="kl-table">
          <el-table v-if="tableShow" :max-height="maxHeight" stripe border :data="tableData" @cell-click="cellClick" class="wuserInfo-table">
            <el-table-column type="index" label="NO." min-width="50" :index="indexMethod" fixed></el-table-column>
            <el-table-column prop="userCode" min-width="100" label="用户编号"></el-table-column>
            <el-table-column prop="userName" min-width="100" label="用户名称"></el-table-column>
            <el-table-column prop="userAddress" min-width="100" label="用户地址" show-overflow-tooltip></el-table-column>
            <el-table-column prop="userStart" min-width="80" label="上期抄码"></el-table-column>
            <el-table-column prop="userEnd" min-width="70" label="本期抄码"></el-table-column>
            <el-table-column prop="userWrite" min-width="90" label="实际用水量"></el-table-column>
            <el-table-column prop="copyData" min-width="100" label="抄表日期"></el-table-column>
            <el-table-column prop="copyState" min-width="80" label="抄表状态"></el-table-column>
            <el-table-column prop="state" min-width="80" label="用户状态"></el-table-column>
            <el-table-column prop="userPeople" min-width="100" label="抄表员"></el-table-column>
            <el-table-column label="操作" fixed="right" width="80px">
              <template slot-scope="scope">
                <el-button type="text" @click="luru()" style="font-size:12px;">详情</el-button>
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
    <div class="Meter-Reading-Record-right-content1" v-else-if='divChange==0'>
      <MeterReadingDataEntry @calback="closeDialog"></MeterReadingDataEntry>
    </div>
    <div class="Meter-Reading-Record-right-content1" v-else-if='divChange==2'>
      <DataCollectionEdit @calback="closeDialog"></DataCollectionEdit>
    </div>
  </div>
</template>
<script>
import MeterReadingDataEntry from "./MeterReadingDataEntry";
import DataCollectionEdit from "./DataCollectionEdit";
export default {
  name: "Meter-Reading-Record-index",
  components: {
    MeterReadingDataEntry,
    DataCollectionEdit
  },
  data() {
    return {
      TableStatusList: [
        {
          value: 1,
          label: "正常"
        },
        {
          value: 2,
          label: "关门"
        },
        {
          value: 3,
          label: "物阻"
        },
        {
          value: 4,
          label: "过圈"
        },
        {
          value: 5,
          label: "自报"
        },
        {
          value: 6,
          label: "估抄"
        },
        {
          value: 7,
          label: "无铅封"
        },
        {
          value: 8,
          label: "不见表"
        },
        {
          value: 9,
          label: "私移表"
        },
        {
          value: 10,
          label: "表不符"
        }
      ],
      tableData: [
        {
          userCode: "CAZZ0001",
          userNum: "1",
          userName: "焦文",
          userAddress: "深圳市东深小区",
          userStart: "915",
          userEnd: "1213",
          copyList: "200",
          addWrite: "198",
          copyData: "2019-07-24",
          copyState: "正常",
          state: "正常",
          userPeople: "高友贵",

          preCopy: "0",
          nowCopy: "986",
          addWrite: "986",
          count: "986",
          preNum: "615",
          preCopyDate: "2019-06-15",
          nowCopyDate: "2019-07-19",
          copyState: "估抄",
          newCode: "CAA0001",
          oldCode: "DZ00001",
          wirteCode: "564",
          preNum: "619",
          preLevel: "10.2%",
          remark: "深圳市东深小区客服营收系统已启用。"
        },
        {
          userCode: "CAZZ0002",
          userNum: "2",
          userName: "焦文",
          userAddress: "深圳市东深小区",
          userStart: "915",
          userEnd: "1213",
          copyList: "200",
          addWrite: "198",
          copyData: "2019-07-24",
          copyState: "正常",
          state: "停用",
          userPeople: "高友贵",

          preCopy: "0",
          nowCopy: "986",
          addWrite: "986",
          count: "986",
          preNum: "615",
          preCopyDate: "2019-06-15",
          nowCopyDate: "2019-07-19",
          copyState: "估抄",
          newCode: "CAA0001",
          oldCode: "DZ00001",
          wirteCode: "564",
          preNum: "619",
          preLevel: "10.2%",
          remark: "深圳市东深小区客服营收系统已启用。"
        },
        {
          userCode: "CAZZ0003",
          userNum: "3",
          userName: "焦文",
          userAddress: "深圳市东深小区",
          userStart: "915",
          userEnd: "1213",
          copyList: "200",
          addWrite: "198",
          copyData: "2019-07-24",
          copyState: "正常",
          state: "销户",
          userPeople: "高友贵",

          preCopy: "0",
          nowCopy: "986",
          addWrite: "986",
          count: "986",
          preNum: "615",
          preCopyDate: "2019-06-15",
          nowCopyDate: "2019-07-19",
          copyState: "估抄",
          newCode: "CAA0001",
          oldCode: "DZ00001",
          wirteCode: "564",
          preNum: "619",
          preLevel: "10.2%",
          remark: "深圳市东深小区客服营收系统已启用。"
        }
      ],
      crumbsData: {
        //面包屑
        titleList: [
          { title: "抄表开账" },
          { title: "抄表" },
          { title: "抄表录入" }
        ]
      },
      treeDatas: {
        tree: [
          {
            foreignkey: "全部",
            id: "222",
            _child: [
              {
                foreignkey: "城区",
                id: "222",
                _child: [
                  { foreignkey: "0035E/100户-未抄0-异常2", id: "222" },
                  { foreignkey: "0036E/100户-未抄10-异常2", id: "222" },
                  { foreignkey: "0037E/80户-未抄0-异常0", id: "222" },
                  { foreignkey: "0038E/99户-未抄1-异常1", id: "222" }
                ]
              }
            ]
          }
        ],
        defaultProps: {
          label: "foreignkey",
          children: "_child"
        },
        inputProp: {
          showSearch: false,
          Inp_placeholder: "请输入节点"
        },
        sendTreeProp: [
          "parentMask",
          "mask",
          "childMask",
          "seq",
          "id",
          "type",
          "foreignkey"
        ],
        rootName: "总部",
        defaultOpen: {
          nodeKey: "id"
          // nodeItem:164
        }
      },
      formData: {
        beginYearM: "201908",
        TableStatus: ""
      },
      dialogFormVisible: false, //弹出表单
      dialogJiChao: false, //集抄弹出表单
      form: {
        name: "",
        region: "",
        date1: "",
        date2: "",
        delivery: false,
        type: [],
        resource: "",
        desc: ""
      },
      formLabelWidth: "120px",
      rules: {
        Applicant: [
          { required: true, message: "请输入申请人", trigger: "blur" }
        ],
        applicantDate: [
          { required: true, message: "请输入申请日期", trigger: "blur" }
        ],
        engineerType: [
          { required: true, message: "请选择工程类型", trigger: "change" }
        ],
        Contacts: [
          { required: true, message: "请输入联系人", trigger: "blur" }
        ],
        documentType: [
          { required: true, message: "请输入证件类型", trigger: "blur" }
        ],
        documentNumber: [
          { required: true, message: "请输入证件号码", trigger: "blur" }
        ],
        contactNumber: [
          { required: true, message: "请输入座机号码", trigger: "blur" }
        ],
        Emergency: [
          { required: true, message: "请选择紧急程度", trigger: "change" }
        ]
      },
      ruleForm: {
        Applicant: "",
        engineerType: ""
      },
      dialogForm: {
        TableStatus: 1,
        changeTables: "1"
      },
      formData1: {
        searchType: "1"
      },
      options6: [
        {
          value: 12,
          label: "用水报装"
        }
      ],
      TableStatusList: [
        {
          value: 1,
          label: "正常"
        },
        {
          value: 2,
          label: "关门"
        },
        {
          value: 3,
          label: "物阻"
        },
        {
          value: 4,
          label: "过圈"
        },
        {
          value: 5,
          label: "自报"
        },
        {
          value: 6,
          label: "估抄"
        },
        {
          value: 7,
          label: "无铅封"
        },
        {
          value: 8,
          label: "不见表"
        },
        {
          value: 9,
          label: "私移表"
        },
        {
          value: 10,
          label: "表不符"
        }
      ],
      histroyData: {},
      tableQuery: {
        //查询表单数据
        code: "",
        fcode: "",
        factoryName: "",
        model: "",
        type: "",
        waterGroup: "",
        areaName: "",
        sn: "",
        connect: "",
        sensorModel: "",
        page: 1,
        pageCount: 20,
        daytime: "",
        meter: "",
        usn: "",
        concentrator: "",
        status: "",
        // unusual:'',
        areaId: ""
      },
      maxHeight: 0,
      tableShow: false,
      tabsData: [
        {
          name1: "11",
          name2: "22",
          name3: false,
          name4: "44",
          name5: "55",
          name6: "66",
          name7: "77",
          name8: "88",
          name9: "99",
          name10: "1010"
        },
        {
          name1: "111",
          name2: "222",
          name3: true,
          name4: "444",
          name5: "555",
          name6: "666",
          name7: "777",
          name8: "888",
          name9: "999",
          name10: "101010"
        },
        {
          name1: "1111",
          name2: "2222",
          name3: true,
          name4: "4444",
          name5: "5555",
          name6: "6666",
          name7: "7777",
          name8: "8888",
          name9: "9999",
          name10: "10101010"
        }
      ],
      treedata: [
        {
          label: "一级 1",
          children: [
            {
              label: "二级 1-1",
              children: [
                {
                  label: "三级 1-1-1"
                }
              ]
            }
          ]
        },
        {
          label: "一级 2",
          children: [
            {
              label: "二级 2-1",
              children: [
                {
                  label: "三级 2-1-1"
                }
              ]
            },
            {
              label: "二级 2-2",
              children: [
                {
                  label: "三级 2-2-1"
                }
              ]
            }
          ]
        },
        {
          label: "一级 3",
          children: [
            {
              label: "二级 3-1",
              children: [
                {
                  label: "三级 3-1-1"
                }
              ]
            },
            {
              label: "二级 3-2",
              children: [
                {
                  label: "三级 3-2-1"
                }
              ]
            }
          ]
        }
      ],
      enterMeterDataShow: false,
      enterMeterDate: "",
      // 日期选择器
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() > Date.now();
        }
      },
      DateChoosevalue: "",
      divChange: 1
    };
  },
  mounted() {
    // 侧边栏
    eventBus.$emit("asideMenuShow", "businessMenuShow3");
    this.init();
    this.$nextTick(() => {
      //data 里面定义一个axHeight 用于设置table的高度，增加一个v-if事件用于渲染吗，监听maxHeight 的变化控制v-if的变量
      this.common.changeTable(
        this,
        ".Meter-Reading-Record .Meter-Reading-Record-right",
        [".Meter-Reading-Record .table-top"]
      );
    });
    let myDate = new Date();
    this.formData.beginYearM =
      myDate.getFullYear() +
      "-" +
      (Number(myDate.getMonth()) + 1) +
      "-" +
      myDate.getDate();
  },
  methods: {
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
      this.intervalGroup = row.intervals;
      if (column.label === "水表编号") {
        this.realTimeData = row;
        if (this.realTimeData.name != null) {
          this.nameRealDate =
            this.$route.query.name +
            "  " +
            this.realTimeData.code +
            "（" +
            this.realTimeData.name +
            "）";
        } else {
          this.nameRealDate =
            this.$route.query.name + "  " + this.realTimeData.code;
        }
        // this.nameRealDate = this.$route.query.name+' '+this.realTimeData.code
        this.$refs.realDateRef.init(this.realTimeData.code);
        this.$refs.realDateRef.getDayData(this.realTimeData.code);
        this.$refs.realDateRef.getChannelType();
        this.$refs.realDateRef.getSearchSelectData();
        this.isForm = true;
      }
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
            waterGroup: null,
            userNo: 112233,
            status: "正常"
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
            waterGroup: null,
            userNo: 132433,
            status: "正常"
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
            waterGroup: null,
            userNo: 1156933,
            status: "容忍"
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
            waterGroup: null,
            userNo: 187433,
            status: "未抄"
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
    add() {
      //添加
      this.dialogFormVisible = true;
    },
    callBack() {
      //返回
      this.crumbsData = {
        //面包屑
        titleList: [
          { title: "抄表开账" },
          { title: "抄表" },
          { title: "抄表录入" }
        ]
      };
      this.divChange = 1;
      this.enterMeterDataShow = false;
    },
    exportExcel() {
      //导出
      let listData = {};
      this.tableQuery.pageCount =
        this.historyData.total === 0 ? 20 : this.historyData.total;
      let params = {
        busicode: "MeterReadingList",
        data: this.tableQuery
      };
      this.$api
        .fetch({
          apiUrl: "interface.api", //路径
          method: "post", //请求方法
          params: params, //参数
          needErrorCallback: true
        })
        .then(res => {
          if (res.code === 0) {
            listData = res.data;
            if (!listData.list[0]) {
              this.$notify({
                title: "警告",
                message: "表格没有相关数据",
                type: "error"
              });
            } else {
              import("@/vendor/Export2Excel").then(excel => {
                const tHeader = [
                  "水表编号",
                  "抄表日期",
                  "所属小区",
                  "厂家编号",
                  "厂家",
                  "型号",
                  "当日读数",
                  "修正读数",
                  "当日水量",
                  "数据状态",
                  "用户编号",
                  "集中器编号",
                  "用户地址"
                ];
                const filterVal = [
                  "code",
                  "daytime",
                  "areaName",
                  "fcode",
                  "factoryName",
                  "model",
                  "lvalue",
                  "fixValue",
                  "flux",
                  "status",
                  "usn",
                  "concentrator",
                  "position"
                ];
                const list = listData.list;
                const data = this.formatJson(filterVal, list);
                excel.export_json_to_excel({
                  header: tHeader,
                  data,
                  filename: this.tableQuery.daytime + "抄表数据",
                  autoWidth: "100"
                });
              });
              this.tableQuery.pageCount = 20;
            }
          }
        });
      this.util.exportExcel();
    },
    luru() {
      //录入
      this.crumbsData = {
        //面包屑
        titleList: [
          { title: "抄表开账" },
          { title: "抄表" },
          { title: "抄表录入" }
          // { title: '录入', method: () => { window.histroy.back() } }
        ]
      };
      this.divChange = 0;
      this.enterMeterDataShow = true;
    },
    jichao() {
      //集抄

      this.divChange = 2;
      this.crumbsData = {
        //面包屑
        titleList: [
          { title: "抄表开账" },
          { title: "抄表" },
          { title: "抄表录入" }
          // { title: '集抄导入', method: () => { window.histroy.back() } }
        ]
      };
      this.dialogJiChao = true;
    },
    edit() {
      this.dialogFormVisible = true;
    },
    // calBack(){

    //   this.enterMeterDataShow  = false;
    // },
    backTreeData() {},
    closeDialog(val) {
      this.enterMeterDataShow = val;
      this.crumbsData = {
        //面包屑
        titleList: [
          { title: "抄表开账" },
          { title: "抄表" },
          { title: "抄表录入" }
        ]
      };
    }
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
$imgWidth: 200px;
$theme-color: #297acc;
.Meter-Reading-Record {
  height: 100%;

  .change-tables-table {
    .el-table__body {
      tr {
        td:nth-child(4) {
          div {
            color: #3195f4;
            cursor: pointer;
          }
        }
      }
    }
  }
  .Meter-Reading-Record-right-content1 {
    height: 100%;
    .kr-right1-top {
      display: flex;
      justify-content: space-between;
      padding: 10px 20px 10px 10px;
    }
    .kr-right1-buttom {
      height: 100%;
      display: flex;
      .luru-right {
        width: 27%;
        border-right: 1.5px solid rgb(234, 244, 255);
        .luru-right-detail {
          padding: 10px 20px;
          .label-detail {
            text-align: right;
            vertical-align: middle;
            float: left;
            font-size: 14px;
            color: #606266;
            line-height: 20px;
            padding: 0 10px 14px 0;
            -webkit-box-sizing: border-box;
            box-sizing: border-box;
          }
          .dialogcontent-detail {
            text-align: right;
            vertical-align: middle;
            float: left;
            font-size: 14px;
            color: #121315;
            line-height: 20px;
            padding: 0 10px 14px 0;
            -webkit-box-sizing: border-box;
            box-sizing: border-box;
          }
        }
        .luru-righ-check {
          // border: 1px solid #e2dddd;
          // margin: 0px 14px;
          // padding: 10px 10px 0px;
          .label-detail {
            text-align: right;
            vertical-align: middle;
            float: left;
            font-size: 14px;
            color: #606266;
            line-height: 20px;
            padding: 0 10px 14px 0;
            -webkit-box-sizing: border-box;
            box-sizing: border-box;
          }
          .dialogcontent-detail {
            text-align: right;
            vertical-align: middle;
            float: left;
            font-size: 14px;
            color: #121315;
            line-height: 20px;
            padding: 0 10px 14px 0;
            -webkit-box-sizing: border-box;
            box-sizing: border-box;
          }
        }
      }
      .luru-left {
        width: 73%;
      }
    }
  }
  .dialog-jichao {
    height: 85%;
    .el-dialog {
      height: 100% !important;
    }
    .el-dialog__body {
      padding: 0px 20px;
      .primaryData {
        margin-top: 10px;
        padding-left: 20px;
        b {
          font-weight: 400;
          color: #000;
        }
        span {
          margin-right: 10px;
        }
      }
      .el-form {
        width: 100%;
        zoom: 1;
        &:after {
          display: block;
          content: "";
          clear: both;
        }
        .form-left {
          .bookNo {
            .el-form-item__label {
              width: 10px;
              text-align: left;
            }
          }
          .el-form-item {
            margin-bottom: 0px;
          }
          & > .el-form-item__content {
            width: 100%;
          }
          .searchBtn {
            margin-left: 1.5rem;
            background: #faa646;
            color: #fff;
            height: 28px;
            margin-top: 0.5rem;
          }
        }
      }
    }
  }
  // .dialog-luru{
  //   .el-dialog__body{
  //     display: flex;
  //     .luru-right{
  //       width: 40%;
  //     }
  //     .luru-left{
  //       width: 60%;
  //     }
  //   }
  // }

  // .Meter-Reading-Record-right-content {
  //   height: 87%;
  //   overflow-y: auto;
  //   margin-top: 1rem;
  //   display: flex;
  //   flex-direction: row;
  //   .kr-left {
  //     width: 18%;
  //     height: 100%;
  //     // border: 2px solid red;
  //   }
  //   .kr-right {
  //     width: 81%;
  //     height: 100%;
  //     table {
  //       overflow: scroll;
  //       width: 100%;
  //       thead th {
  //         width: 10%;
  //         font-size: 14px;
  //         // font-weight: 700;
  //       }
  //       tbody tr {
  //         // display:block;
  //         margin-top: 1rem;
  //         td {
  //           text-align: center;
  //           .el-input,
  //           .el-select {
  //             width: 95%;
  //           }
  //         }
  //       }
  //     }
  //   }
  // }
  .btnBox {
    display: inline-block;
    width: 100%;
    text-align: right;
    margin-bottom: 2%;
  }
  .btnBox .el-button {
    margin-left: 1%;
    font-size: 14px;
  }
  .my-input {
    width: 90%;
    display: inline-block;
    // padding-left: 10%;
    .el-input__inner {
      width: 100% !important;
    }
    .el-form-item {
      margin-bottom: 0px;
    }
  }
  .my-input .el-input {
    width: 100% !important;
  }
  .my-input .el-select .el-input {
    width: 100%;
  }
  .my-important {
    .el-form-item__label {
      color: red;
      font-weight: bolder;
    }
  }
  .dialogcontent {
    width: 100%;
    display: inline-block;
  }
  .dialogcontent span {
    width: 100%;
    display: inline-block;
    // border-bottom: 1px solid;
  }
  .dialogcontent1 {
    width: 58%;
    display: inline-block;
  }
  .inputcontent {
    width: 52%;
    display: inline-block;
    // border-bottom: 1px solid;
  }
  .dialogcontent2 {
    width: 58%;
    display: inline-block;
  }
  .dialogcontent3 {
    width: 40%;
    display: inline-block;
  }
  .dialogcontent3 .inputcontent {
    width: 100%;
    display: inline-block;
    // border-bottom: 1px solid;
  }
  .kl-table {
    padding: 0 20px;
    .block {
      text-align: right;
      padding: 10px 0px;
    }
  }
  .longinput .el-input__inner {
    width: 160px;
  }
}
</style>

