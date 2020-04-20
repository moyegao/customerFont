
<template>
  <div class="DataCollectionEdit">
    <el-dialog class="button-dialog" title="用水分析" :visible.sync="centerDialogVisible" width="60%" center>
      <span></span>
      <span slot="footer" class="dialog-footer">
        <el-button size="mini" @click="centerDialogVisible = false">取 消</el-button>
        <el-button size="mini" type="primary" @click="centerDialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
    <div class="toolbar">
      <el-form :inline="true" size="mini" :model="formData" class="demo-form-inline">
        <el-form-item class="form-left">
          <el-form-item label="册本号：">
            <el-input v-model="formData.con" placeholder=""></el-input>
          </el-form-item>
          <el-form-item label='抄表日期：'>
            <el-date-picker v-model="DateChoosevalue" type="date" value-format="yyyy-MM-dd" :picker-options="pickerOptions" :clearable="true">
            </el-date-picker>
          </el-form-item>
          <el-form-item label="最大容忍天数：">
            <el-input v-model="formData.date" placeholder="5"></el-input>
          </el-form-item>
          <el-form-item label="是否异常：">
            <el-select clearable v-model="formData.isError" placeholder="是否异常">
              <el-option v-for="(item,index) in formData.isErrorOptions" :key="index" :label="item.name" :value="item.value"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item>
            <el-button size="mini" type="primary">抄读</el-button>
            <el-button size="mini" type="primary">查看实时数据</el-button>
          </el-form-item>
          <br>
        </el-form-item>
        <el-form-item class="form-right">
        </el-form-item>
      </el-form>
      <h4 class="primaryData">
        <span>总数：<b>{{Wtotal}}</b></span>
        <span>未抄：<b>{{notCopied}}</b></span>
        <span>已抄：<b>{{Copied}}</b></span>
      </h4>
    </div>

    <div class="kl-table">
      <el-table stripe border :data="histroyData.list" class="change-tables-table" @cell-click="cellClick">

        <el-table-column type="selection" width="50" fixed="left">
        </el-table-column>

        <el-table-column type="index" label="NO." width="50" fixed="left" :index="indexMethod">
        </el-table-column>

        <el-table-column prop="iserror" min-width="80" label="是否异常">
        </el-table-column>

        <el-table-column prop="connect" min-width="80" label="用户名称">
        </el-table-column>

        <el-table-column prop="userNo" min-width="100" label="用户编号">

        </el-table-column>

        <el-table-column prop="lvalue" min-width="100" label="装表时间">
        </el-table-column>

        <el-table-column prop="connect" min-width="70" label="上期抄表日">
        </el-table-column>

        <el-table-column prop="fixValue" min-width="80" label="已抄日期">
        </el-table-column>

        <el-table-column prop="flux" min-width="80" label="已抄水量">
        </el-table-column>

        <el-table-column prop="fixValue" min-width="80" label="抄表日期">
        </el-table-column>

        <el-table-column prop="flux" min-width="80" label="止码">
        </el-table-column>

        <el-table-column prop="flux" min-width="80" label="水量">
        </el-table-column>

        <el-table-column prop="lvalue" min-width="100" label="用户地址">
        </el-table-column>

      </el-table>
    </div>
  </div>
</template>
<script>
export default {
  name: "DataCollectionEdit",
  data() {
    return {
      centerDialogVisible: false,
      histroyData: {
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
        ]
      },
      formData: {
        con: "",
        date: "",
        isError:'',
        isErrorOptions:[],
      },
      // 日期选择器
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() > Date.now();
        }
      },
      DateChoosevalue: "",
      Wtotal: 455,
      disparity: 5,
      notCopied: 400,
      Copied: 395,
      backfillSubmitForm: {
        linkTel: "",
        linkMan: "",
        state: "0",
        bankCode: "",
        desc: "",
        areaName: ""
      }
    };
  },
  methods: {
    cellClick(row, column, cell, event) {
      //点击文件名
      let that = this;
      this.intervalGroup = row.intervals;
      if (column.label === "用户编号") {
        this.centerDialogVisible = true;
      }
    }
  }
};
</script>
<style lang="scss">
@import "../../../assets/styles/scss/base/fn";
$imgWidth: 200px;
$theme-color: #297acc;
.DataCollectionEdit {
  height: 100%;
  margin: 0 auto;
  .primaryData {
    display: inline-block;
    height: 100%;
    line-height: 31px;
    b {
      font-weight: 400;
      color: #b34444;
    }
    span {
      margin-right: 10px;
      font-size: 14px;
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
</style>
