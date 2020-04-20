<template>
    <!-- 优惠用水变更 -->
    <div   class="PreferentialWater">
        <el-form  class="formBill" size="mini" :inline="true" :model="PreferentialWaterForm" label-width="130px" ref="ruleForm" :rules="rules">
          <!-- <legend class="legendColumn">用水类型</legend> -->
             <el-form-item label="优惠策略：">
                <el-select clearable v-model="PreferentialStrategy" placeholder="" @change="getPreferentialStrategyoptions">
                <el-option v-for="(item,index) in PreferentialWaterUseEditForm.PreferentialStrategyoptions" :key="index" :label="item.name" :value="item.value"></el-option>
                </el-select>
            </el-form-item>

            <el-form-item label="优惠证件编号：">
                <el-input v-model="PreferentialWaterUseEditForm.SubsistenceAllowances" placeholder=""></el-input>
            </el-form-item>

            <el-form-item label="开始日期：">
                <el-date-picker v-model="PreferentialWaterUseEditForm.year" type="date" placeholder="选择日期">
                </el-date-picker>
            </el-form-item>

            <el-form-item label="结束日期：">
                <el-date-picker v-model="PreferentialWaterUseEditForm.descc" type="date" placeholder="选择日期">
                </el-date-picker>
            </el-form-item>
            
            <el-form-item label="备注："  class="Remarks">
                <el-input type="textarea" :rows="2" v-model="PreferentialWaterUseEditForm.Remarks" placeholder=""></el-input>

            </el-form-item>

        </el-form>
    </div>
    
</template>
<script>
export default {
  name: "PreferentialWater",
  data() {
    return {
      tabsName:"after",
      PreferentialWaterForm:{},
      PreferentialStrategy:'',
      PreferentialWaterUseEditForm: {

        PreferentialStrategyoptions: [
          {
            name:'优惠策略一',
            value:'1'
          },
          {
            name:'优惠策略二',
            value:'2'
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
    getPreferentialStrategyoptions(){},
  }
}
</script>
<style lang="scss">
.PreferentialWater {
}
</style>