<template>
  <div class="CustomerChangeAdd">
    <!-- <commonPart :parentToChild="'2'"></commonPart> -->
    <el-form size="mini" class="commonPartFormTwo commonPartForm " :inline="true" :model="commonPartForm" label-width="130px" ref="ruleForm" :rules="rules">
      <el-tooltip class="item" effect="dark" content="请输入户号/账号/客户编号/手机号/身份证号" placement="top">
        <el-input class="keyword-input" v-model=" commonPartForm.ctmNo" placeholder=""></el-input>
      </el-tooltip>
      <div class="keyword-bottom">
        <el-button size="mini" class='searchBtn' @click="demand" icon="el-icon-search"></el-button>
      </div>

      <el-form-item label="单据编号：">
        <el-input class="read-only" disabled="disabled" v-model=" commonPartForm.ctmNo" placeholder="01003152"></el-input>
      </el-form-item>
      <el-form-item label="单据日期：">
        <el-input class="read-only" disabled="disabled" v-model=" commonPartForm.ctmName" placeholder="2019年11月21日"></el-input>
      </el-form-item>

      <legend class="legendColumn">用户信息</legend>
        <el-form-item label="客户编号：">
          <el-input class="read-only" disabled="disabled" v-model=" commonPartForm.ctmNo" placeholder="01003152"></el-input>
        </el-form-item>
        <el-form-item label="客户名称：">
          <el-input class="read-only" disabled="disabled" v-model=" commonPartForm.ctmName" placeholder="谢顺新"></el-input>
        </el-form-item>
        <el-form-item label="客户地址：">
          <el-input class="read-only" disabled="disabled" v-model=" commonPartForm.ctmAddr" placeholder="宝积山路、体育巷"></el-input>
        </el-form-item>
        <el-form-item label="用户编号：">
          <el-input class="read-only" disabled="disabled" v-model=" commonPartForm.ctmNo" placeholder="01003152"></el-input>
        </el-form-item>
        <el-form-item label="证件号码：">
          <el-input class="read-only" disabled="disabled" v-model=" commonPartForm.mobile" placeholder="444210199909100030092"></el-input>
        </el-form-item>

    </el-form>
    
    <el-tabs type="border-card" @tab-click="tabClick" class="tabsBlock" >
      <el-tab-pane label="用户资料变更">
        <span slot="label"><i class="el-icon-user"></i> 用户资料变更</span>
        <!-- :parentToChild="'3'"  :isEdit="isEdit" -->
        <userInfo :commonPart.sync="commonPart"></userInfo>
      
      </el-tab-pane>
      <el-tab-pane label="用水类型变更">
        <span slot="label"><i class="el-icon-user"></i> 用水类型变更</span>
          <typesOfWater></typesOfWater>
      </el-tab-pane>
      <el-tab-pane label="用水人口变更">
        <span slot="label"><i class="el-icon-user"></i> 用水人口变更</span>
          <wPopulation></wPopulation>
      
      </el-tab-pane>
      <el-tab-pane label="优惠用水变更">
        <span slot="label"><i class="el-icon-user"></i> 优惠用水变更</span>
        <PreferentialWater></PreferentialWater>
      </el-tab-pane>
      <el-tab-pane label="缴费方式变更">
        <span slot="label"><i class="el-icon-user"></i> 缴费方式变更</span>
        <PaymentM></PaymentM>
      
      </el-tab-pane>
      <el-tab-pane label="开票资料变更">
        <span slot="label"><i class="el-icon-user"></i> 开票资料变更</span>
        <InvoiceInformation></InvoiceInformation>
      </el-tab-pane>
      <el-tab-pane label="客户资料变更">
        <span slot="label"><i class="el-icon-user"></i> 客户资料变更</span>
        <customerManagementEdit></customerManagementEdit>
      </el-tab-pane>
      <!-- <el-tab-pane label="变更事项">
        <span slot="label"><i class="el-icon-user"></i> 变更事项</span>
      </el-tab-pane> -->
    </el-tabs>

    <legend class="legendColumn">变更事项</legend>
    <changeMatters class=""></changeMatters>

    <legend class="legendColumn">上传附件</legend>
    <uploadFile ></uploadFile>
    
    <legend class="legendColumn">流程处理</legend>
    <procedure ></procedure>

  </div>
</template>
<script>
import commonPart from "./commonPart"//公告模块
import userInfo from "@/components/userInfo/ctmInfo.vue"//用户资料变更
import typesOfWater from "./typesOfWater"//用水类型变更
import wPopulation from "./wPopulation"//用水类型变更
import PreferentialWater from "./PreferentialWater"//优惠用水变更
import PaymentM from "./PaymentM"//缴费方式变更
import InvoiceInformation from "./InvoiceInformation"//开票资料变更

import uploadFile from '@/components/uploadPic';//上传附件
import procedure from "@/components/procedure"//审批
import changeMatters from "@/components/changeMatters"//变更事项
import customerManagementEdit from '@/views/CustomerCenter/customerManagement/customerManagementEdit'//客户资料变更

export default {
  name: "CustomerChangeAdd",
  props:['id'],
  components: {
    uploadFile,
    customerManagementEdit,
    procedure,
    changeMatters,
    commonPart,
    typesOfWater,
    wPopulation,
    userInfo,
    PreferentialWater,
    PaymentM,
    InvoiceInformation,
  },
  data() {
    return {
      rules:{

      },
      commonPart: {
        parentToChild: '4',
        isEdit: false,
        ctmEdit:false,
        accEdit:false,
        legendTwo:'',
      },
      commonPartForm:{},

      isEdit:false,
      tovalue:'1',
      EditVisible:'tab-1',
      tableData:[
        {
          bookName:"--",
          jihuaNum:'100',
          chaojianNum:'50',
          jihuaKNum:'100',
          kaiZedNum:'60',
          lastPeople:'焦文',
          lastTime:'2019/07/26',
          state:'未开账',
          leveled:'60%',
        },
      ],
      options6: [
        {
          value: 0,
          label: '全部'
        },
        {
          value: 1,
          label: '未开账'
        },
        {
          value: 2,
          label: '已开账'
        },

      ],
      options7go: '',
      options7: [
        {
          value: 0,
          label: '每月抄'
        },
        {
          value: 1,
          label: '奇月抄'
        },
        {
          value: 2,
          label: '偶月抄'
        },
        {
          value: 3,
          label: '季度抄'
        },

      ],
      crumbsData: {  //面包屑
        titleList: [
          { title: '业务管理', path: '/MeterReadingCenter' },
          { title: '抄表开账',  method: () => { window.histroy.back() } },
          { title: '开账', method: () => { window.histroy.back() } },
          { title: '按册开账', method: () => { window.histroy.back() } }
        ],

      },
      isActive: false,  //控制高级查询内容的显示
      formData: {
        con: "",
        beginNumber:"全部",
        watermeterWarehouse: "",
        watermeterWarehouseOptions: [],
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
      waterAddVisible: false,  //综合查询的显示和隐藏
      WaterEditorName: '综合查询',
      ruleFormData: {},
      backfillName: "用户基础资料管理-编辑",
      backfillVisible: false,
    }
  },
  mounted() {
    this.init();
    //data 里面定义一个axHeight 用于设置table的高度，增加一个v-if事件用于渲染吗，监听maxHeight 的变化控制v-if的变量

    this.common.changeTable(this, '.CustomerChangeAdd .kl-table', ['.CustomerChangeAdd .toolbar', '.CustomerChangeAdd .block', '.CustomerChangeAdd bread-contain'])
  },
  methods: {
    init() {
      let _this = this
      let id = _this.id;
      let data = {}
      let params = {}
      if (!_this.id) {
        _this.commonPartForm.billNo = '';
        _this.commonPartForm.billDate = '';
        _this.commonPart.id = id;
        _this.commonPart.userOpenInfo = {
          billNo:'',
        };
        _this.commonPart.userInfo = {
          userNo:'',
          lockFlag:'9',
        };
        _this.commonPart.ctmInfo = {
          certNo:'',
        };
        _this.commonPart.accountInfo = {
          accountNo:'',
        };
        _this.commonPart.isEdit = false;

        _this.commonPart.desc1 = 1;
        _this.commonPartFormShow = true;
        _this.userInfoFormShow = true;
        console.log(_this.commonPart);
        return
      } else {
        data = {
          id,
        }
        params = {
          busicode: "OpenAccQuery",
          data,
        }
        console.log(_this.commonPart);
      }
      _this.$api.fetch({
        params,//参数
      }).then(res => {
        // 父页面获取到值
        _this.commonPartForm.billNo = res.userOpenInfo.billNo;
        _this.commonPartForm.billDate = res.userOpenInfo.billDate;
        _this.commonPartForm.processState = res.userOpenInfo.processState;
        _this.commonPartForm.userCtmTempId = res.userOpenInfo.userCtmTempId;
        _this.commonPartForm.userAccountTempId = res.userOpenInfo.userAccountTempId;
        _this.commonPartForm.userInfoTempId = res.userOpenInfo.userInfoTempId;
        if(res.userOpenInfo.processState == null || res.userOpenInfo.processState == ''){
          _this.$store.state.isRemove = true
        }
        _this.commonPart.id = id;
        _this.commonPart.userOpenInfo = res.userOpenInfo;
        _this.commonPart.userInfo = res.userInfo;
        _this.commonPart.ctmInfo = res.ctmInfo;
        _this.commonPart.accountInfo = res.accountInfo;
        _this.commonPart.updateAccountFlag = res.updateAccountFlag;
        _this.commonPart.updateCtmFlag = res.updateCtmFlag;

        if (res.updateCtmFlag == 0) {
          _this.commonPart.ctmEdit = true;
        }else{
          _this.commonPart.ctmEdit = false;
        }

        if (res.updateAccountFlag == 0) {
          _this.commonPart.accEdit = true;
        }else{
          _this.commonPart.accEdit = false;
        }

        _this.commonPart.desc1 = 1;
        _this.commonPartFormShow = true;
        _this.userInfoFormShow = true;
      })
    },
    tabClick(tab, event){
      this.$set(this.crumbsData.titleList,"3",{title:tab.label,method:()=>{window.histroy.back()}});

      if(tab.label == '按册开账'){
        this.EditVisible = true
      }else{
        this.EditVisible = false
      }
    },
    demand() {  //查询
      // this.tableQuery.page = 1
      // this.tableQuery.pageCount = 20
      this.init()
    },
    indexMethod(index) {//获取表格序号
      //    return  (this.tableQuery.page-1)*this.tableQuery.pageCount + (index+1) ;
      return (this.histroyData.pageSize - 20) * this.histroyData.pages + (index + 1);

    },
    closeDialog() { //关闭会话
      this.waterAddVisible = false;
      this.backfillVisible = false;
      this.init()
    },
    //弹出框
    submit(formName) {	//提交
      this.$refs.childWaterEditor.submit()
      // this.waterAddVisible = false;
    },
    // 添加
    add() {

    },
    // 综合查询
    ComprehensiveQuery() {
      this.waterAddVisible = true;
    },
    // 编辑
    edit() {
      this.backfillVisible = true;
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
.CustomerChangeAdd {
  width:100% ;
  height: 100%;
  overflow-y: auto;
  .el-card{
    width: 100%;
    height: 140px;
    margin: 5px 0px;
    .el-card__header{
      font-weight: 700;
      padding: 5px 20px;
      border-bottom: 1px solid #EBEEF5;
      -webkit-box-sizing: border-box;
      box-sizing: border-box;
      font-size: 14px;
    }
    .item {
      margin-bottom: 5px;
      font-size: 12px
    }

    .clearfix:before,
    .clearfix:after {
      display: table;
      content: "";
    }
    .clearfix:after {
      clear: both
    }
  }

  .tabsBlock{    
    -webkit-box-shadow: none !important;
    box-shadow: none !important;
    margin-top: 10px;
    .el-tabs__header{
      margin: 0 0 5px;
    }
    .el-tabs--border-card{
      border: 1px solid #eaf4ff;
    }
    .el-tabs__content{
      padding: 5px;
      height: calc(100% - 60px);
      .idCard{
        border: 0px;
      }
      .el-tab-pane{
        height: 100%;
      }

    }
  }
}
</style>
