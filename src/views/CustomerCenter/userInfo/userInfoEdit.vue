<template>
  <div class="userInfoEdit">
    <!-- :parentToChild="'2'" :isEdit="isEdit" -->
    <el-form class="formBill commonPartFormTwo commonPartForm " size="mini" :inline="true" :model="TransferEditForm" label-width="130px" ref="ruleForm" :rules="rules">
      <userInfo :commonPart="commonPart" v-if="userInfoShow" ref="userInfo"></userInfo>
    </el-form>
  </div>

</template>
<script>
import userInfo from "@/components/userInfo/ctmInfo.vue"
export default {
  name: "userInfoEdit",
  components: {
    userInfo
  },
  props: ['formData'],
  data() {
    return {
      commonPart: {
        parentToChild: '2',
        // parentToChild: '4',
        // legendTwo: '2',

        isEdit: true,
        userEdit:true,
      },
      TransferEditForm:{
        billNo:'',
        billDate:'',
        transferDate:'',
        transferComments:'',
      },
      userInfoShow: false,
      rules: {
        transferDate: [
          { required: true, message: '请输入过户日期', trigger: 'blur' },
        ],
        transferComments: [
          { required: true, message: '请输入过户原因', trigger: 'blur' },
        ]
      },
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    init() {
      let _this = this
      let data = {
        userNo: this.formData.userNo,
      }
      let params = {
        "busicode": "UserInfoQuery",
        data,
      }
      this.$api.fetch({
        params,//参数
      }).then(res => {
        _this.commonPart.userInfo = res.userInfo;
        _this.commonPart.accountInfo = res.accountInfo;
        _this.commonPart.ctmInfo = res.ctmInfo;
        _this.commonPart.msInfo = res.msInfo;
        _this.userInfoShow = true;
      })
    },
    toParentData() {
      let data = this.$refs.userInfo.toParentData()
      data.userNo = this.formData.userNo;
      return data;
    },
  },
}
</script>
<style lang="scss">
.userInfoEdit {
  width: 100%;
  height: calc(100% - 40px);
  overflow: auto;
  overflow-y: auto;
}
</style>

