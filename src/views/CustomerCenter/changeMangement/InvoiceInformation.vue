<template>
      <!-- 开票资料变更 -->
    <div   class="InvoiceInformation">
        <el-form  class="formBill" size="mini" :inline="true" :model="InvoiceInformationForm" label-width="130px" ref="ruleForm" :rules="rules">
            <el-form-item label="发票类型：" prop="desc">
                <el-input v-model="InvoiceInformationForm.desc" placeholder=""></el-input>
            </el-form-item>
            <el-form-item :label="v.label" v-for="(v,i) in InvoiceInformationForm.InvoiceInformation" :key="i">
                <el-input v-model="v.value" :placeholder="v.tips"></el-input>
            </el-form-item>
            <el-form-item label="开户行：">
                <el-select clearable v-model="InvoiceInformationForm.year" placeholder="">
                <el-option v-for="(item,index) in InvoiceInformationForm.bankName" :key="index" :label="item.name" :value="item.value"></el-option>

                </el-select>
            </el-form-item>
            <el-form-item label="银行账号：" class="">
                <el-input v-model="InvoiceInformationForm.bankCountNum" placeholder="">
                <el-button slot="append" icon="el-icon-reading"  type="primary" class="scanning-btn" size='mini'></el-button>
                </el-input>
            </el-form-item>
            <el-form-item label="投递方式：">
                <el-select clearable v-model="InvoiceInformationForm.year" placeholder="自取">
                <el-option v-for="(item,index) in InvoiceInformationForm.yyy" :key="index" :label="item.name" :value="item.value"></el-option>

                </el-select>
            </el-form-item>
        </el-form>
    </div>
    
</template>
<script>
export default {
  name: "InvoiceInformation",
  data() {
    return {
      tabsName:"after",
      InvoiceInformationForm: {

        
        InvoiceInformation: [
          {
            label: "单位名称：",
            value: "",
            tips: ""
          },
          {
            label: "纳税人识别号：",
            value: "",
            tips: "12441400714861777E "
          },
          {
            label: "联系地址：",
            value: "",
            tips: ""
          },
          {
            label: "座机号码：",
            value: "",
            tips: ""
          },


        ],
        PriceStrategy: "",
        isfines: "",
        UpdateCodeCopye: "",
        Remarks: "",
        year: ""
      },
      rules: {
        userNume: [
          { required: true, message: '请输入用户编号', trigger: 'blur' },
        ],
        desc: [
          { required: true, message: '请输入发票类型', trigger: 'blur' },
        ]
      },
      histroyData: {},

    }
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {

    },

  }
}
</script>
<style lang="scss">
.InvoiceInformation {
}
</style>