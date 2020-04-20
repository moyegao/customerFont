<template>
  <!-- 缴费方式变更 -->
  <div class="PaymentM">
    <el-form class="formBill" size="mini" :inline="true" :model="PaymentMethodAddForm" label-width="130px" ref="ruleForm" :rules="rules">
      <!-- <legend class="legendColumn">用水类型</legend> -->
      <el-form-item label="账户编号：">
        <el-input class="read-only" disabled="disabled" v-model="PaymentMethodAddForm.PriceStrategy" placeholder="101801012001"></el-input>
      </el-form-item>
      <el-form-item label="账户类型：">
        <el-select clearable v-model="PaymentMethodAddForm.aaaa" placeholder="">
          <el-option v-for="(item,index) in PaymentMethodAddForm.aaaas" :key="index" :label="item.name" :value="item.value"></el-option>

        </el-select>
      </el-form-item>
      <el-form-item label="缴费方式：">
        <el-select clearable v-model="PaymentMethodAddForm.isfines" placeholder="">
          <el-option v-for="(item,index) in PaymentMethodAddForm.PaymentMethod" :key="index" :label="item.name" :value="item.value"></el-option>

        </el-select>
      </el-form-item>
      <el-form-item label="余额：">
        <el-input v-model="PaymentMethodAddForm.isfines1" placeholder="10000"></el-input>
      </el-form-item>
      <el-form-item label="开户银行：">
        <el-select clearable v-model="PaymentMethodAddForm.UpdateCodeCopye" placeholder="">
          <el-option v-for="(item,index) in PaymentMethodAddForm.bankName" :key="index" :label="item.name" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="开户名称：">
        <el-input v-model="PaymentMethodAddForm.UpdateCodeCopye1" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="银行账户：" class=''>
        <el-input v-model="PaymentMethodAddForm.UpdateCodeCopye2" placeholder="">
          <el-button slot="append" icon="el-icon-reading" type="primary" class="scanning-btn" size='mini'></el-button>
        </el-input>
      </el-form-item>
      <el-form-item label="托号：">
        <el-input v-model="PaymentMethodAddForm.UpdateCodeCopye1" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="托收合同号：">
        <el-input v-model="PaymentMethodAddForm.UpdateCodeCopye1" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="托收类型：">
        <el-select clearable v-model="PaymentMethodAddForm.UpdateCodeCopye" placeholder="">
          <el-option v-for="(item,index) in PaymentMethodAddForm.tuoshouType" :key="index" :label="item.name" :value="item.value"></el-option>
        </el-select>
      </el-form-item>

      <el-form-item label="备注：" class="Remarks">
        <el-input v-model="PaymentMethodAddForm.remark" autocomplete="off" type="textarea" placeholder="请输入备注"></el-input>
      </el-form-item>

    </el-form>
  </div>

</template>
<script>
export default {
  name: "PaymentM",
  data() {
    return {
      tabsName: "after",
      PaymentMethodAddForm: {
        tuoshouType:[
          {
            name: "小额",
            value: '1'
          },
          {
            name: "单位",
            value: '2'
          },
          {
            name: "金融",
            value: '3'
          },
        ],
        descoptions: [
          {
            name: "居民用水",
            value: '2'
          },
          {
            name: "特种用水",
            value: '4'
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
.PaymentM {
}
</style>