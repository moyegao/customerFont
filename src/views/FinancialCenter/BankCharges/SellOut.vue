
<template>
  <div class="SellOut">
    <div>
      <el-form class="formBill-One" :inline="true" :model="formData">

        <el-form-item label="收费类型：">
          <el-select clearable v-model="formData.waterMeterCaliber" placeholder="">
            <el-option v-for="(item,index) in formData.waterMeterManufacturerOptions" :key="index" :label="item.name" :value="item.value"></el-option>
          </el-select>
        </el-form-item>

        <el-form-item label="出盘银行：">
          <el-select v-model="formData.con" placeholder="请选择">
            <el-option v-for="item in formData.bankOptions" :key="item.value" :label="item.name" :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>

        <el-form-item label='开始账期：'>
          <el-date-picker v-model="DateChoosevalue" type="date" placeholder="选择日期">
          </el-date-picker>
        </el-form-item>

        <el-form-item label='截止账期：'>
          <el-date-picker v-model="formData.watermeterWarehouse" type="date" placeholder="选择日期">
          </el-date-picker>
        </el-form-item>

      </el-form>
    </div>
  </div>
</template>
<script>
export default {
  name: "SellOut",
  data() {
    return {
      formData: {
        con: "",
        watermeterWarehouse: "",
        watermeterWarehouseOptions: [
          {
            name: "全部",
            value: "1"
          },
          {
            name: "未出盘",
            value: "2"
          },
          {
            name: "已出盘",
            value: "3"
          }
        ],
        waterMeterManufacturer: "",
        waterMeterManufacturerOptions: [
          {
            name: "银行代扣",
            value: "1"
          },
          {
            name: "银行托收",
            value: "2"
          }
        ],
        waterMeterType: "",
        waterMeterTypeOptions: [
          {
            name: "全部",
            value: "4"
          },
          {
            name: "未销账",
            value: "5"
          },
          {
            name: "已销账",
            value: "6"
          }
        ],
        waterMeterCaliber: "",
        waterMeterCaliberOptions: [],
        waterMeterStatus: "",
        waterMeterStatusOptions: [],
        subordinateDepartments: "",
        subordinateDepartmentsOptions: [],
        bankOptions: [
          {
            name: "中国银行",
            value: "1"
          }
        ]
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
      histroyData: {},

      TypesOfWaterUseVisible: false, //综合查询的显示和隐藏
      TypesOfWaterUseName: "详情"
    };
  },
  mounted() {
    this.init();
    //data 里面定义一个axHeight 用于设置table的高度，增加一个v-if事件用于渲染吗，监听maxHeight 的变化控制v-if的变量

    this.common.changeTable(this, ".water-meter-inquiry .kl-table", [
      ".water-meter-inquiry .toolbar",
      ".water-meter-inquiry .block",
      ".water-meter-inquiry #crumbs"
    ]);
  },
  methods: {
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
            areaName: "八顷街43号",
            code: "C0101237",
            concentrator: "",
            connect: null,
            daytime: "梁锶薇",
            endTime: 3001,
            factoryName: "东井",
            fcode: 190,
            fix: false,
            fixValue: "13535032961",
            flux: "030501横沥农信",
            getMeter: 622439880017564557,
            id: null,
            isxz: "9.24",
            lvalue: "-",
            meter: 1,
            model: "N",
            modifyBy: "A",
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
            areaName: "沙头西街182号",
            code: "C0101231",
            concentrator: "",
            connect: null,
            daytime: "邝顺荣",
            endTime: 3001,
            factoryName: "东井",
            fcode: 190,
            fix: false,
            fixValue: "15920519806",
            flux: "030501横沥农信",
            getMeter: 622439880037037931,
            id: null,
            isxz: "19.24",
            lvalue: "-",
            meter: 2,
            model: "N",
            modifyBy: "A",
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
    handleClick(tab, event) {},
    demand() {
      //查询
      // this.tableQuery.page = 1
      // this.tableQuery.pageCount = 20
      this.init();
    },
    //   导出
    exportExcel() {
      //   	let listData = {}
      //   	this.tableQuery.pageCount = this.historyData.total ===0? 20:this.historyData.total
      //     let params ={
      //       "busicode":"MeterReadingList",
      //       "data": this.tableQuery
      //     }
      //     this.$api.fetch({
      //       apiUrl: 'interface.api',//路径
      //       method: 'post',//请求方法
      //       params:params,//参数
      //       needErrorCallback:true
      //     }).then(res =>{
      //     	if(res.code===0){
      //         listData = res.data
      //         if (!listData.list[0]) {
      //         		this.$notify({
      //               title: '警告',
      //               message:'表格没有相关数据',
      //               type: 'error'
      //             })
      //         	} else {
      //         		import('@/vendor/Export2Excel').then(excel => {
      // 			        const tHeader = ['水表编号', '抄表日期', '所属小区','厂家编号','厂家','型号', '当日读数', '修正读数', '当日水量', '数据状态',  '用户编号','集中器编号','用户地址']
      // 			        const filterVal = ['code', 'daytime', 'areaName','fcode','factoryName','model','lvalue', 'fixValue','flux', 'status', 'usn', 'concentrator', 'position']
      // 			        const list = listData.list
      // 			        const data = this.formatJson(filterVal, list)
      // 			        excel.export_json_to_excel({
      // 			          header: tHeader,
      // 			          data,
      // 			          filename: this.tableQuery.daytime + '抄表数据',
      // 			          autoWidth: '100'
      // 			        })
      // 			      })
      //         		this.tableQuery.pageCount = 20
      //         	}
      //       }
      //     })
    },
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
      this.TypesOfWaterUseVisible = false;
    },
    SellOut() {
      this.TypesOfWaterUseVisible = true;
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
.SellOut {
  width: 100%;
  .el-form-item--mini.el-form-item,
  .el-form-item--small.el-form-item {
    margin-bottom: 8px;
  }
}
</style>