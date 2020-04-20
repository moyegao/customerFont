<template>
    <!-- 用水人口变更 -->
    <div class="wPopulation">
        <el-form class="formBill" size="mini"  :inline="true" :model="wPopulation" label-width="130px" ref="ruleForm" >
          <!-- <legend class="legendColumn">用水人口</legend> -->
            <el-form-item label="用水人数：" style="margin-left: -3px">
                <el-input v-model="wPopulation.userNum" disabled="disabled" placeholder=""></el-input>
            </el-form-item>
            <el-form-item label="备注："   class="Remarks">
                <el-input type="textarea" :rows="2" v-model="wPopulation.year" placeholder=""></el-input>
            </el-form-item>
            <el-table      
                stripe
                border
                :data="histroyData.list"
                class="change-tables-table">

                <el-table-column 
                    type="index"  label="NO."
                    width="50" 
                    fixed="left"
                    :index="indexMethod">
                     
                </el-table-column>

                <el-table-column
                    prop="connect"
                    
                    
                    label="用户姓名">
                    <template slot-scope="scope">
                        <el-input v-model="scope.row.connect1"></el-input>
                    </template>
                </el-table-column>
                    
                <el-table-column
                    prop="connect"
                    
                    label="证件类型">
                    <template slot-scope="scope">
                        <el-select  clearable v-model="scope.row.connect" placeholder="">
                            <el-option  v-for="(item,index) in wPopulation.descoptions" :key="index" :label="item.name" :value="item.value"></el-option>
                        </el-select>
                    </template>
                </el-table-column>

                <el-table-column
                    prop="connect"
                    min-width="130"
                    label="证件号码">
                    <template slot-scope="scope">
                            <el-input @change='checkId(scope.row.connect2)' placeholder="证件号码" v-model="scope.row.connect2" class="input-with-select" >
                                <i slot="suffix" v-if="idCardCheck == 1" size="medium" class="el-input__icon el-icon-success" style=" color: #67c23a;"></i>
                                <i slot="suffix"  size="medium" v-else-if="idCardCheck == 2" class="el-input__icon el-icon-error" style=" color: red;"></i>

                                <el-button  slot="append" class="scanning-btn" icon="el-icon-reading"  type="primary"  @click="idCard()" size='mini'></el-button>
                            </el-input>
                    </template>

                </el-table-column>

                <el-table-column
                    prop="connect"
                    
                    label="座机号码">
                    <template slot-scope="scope">
                        <el-input v-model="scope.row.connect3"></el-input>
                    </template>
                </el-table-column>

                <el-table-column
                    prop="connect"
                    min-width="180"
                    label="备注">
                    <template slot-scope="scope">
                        <el-input v-model="scope.row.connect4"></el-input>
                    </template>
                </el-table-column>



                </el-table>

        </el-form>
    </div>
    
</template>
<script>
export default {
    name: "wPopulation",
    data() {
        return {
            idCardCheck:0,    
            wPopulation:{
                // 查询内容
                backfillInput:[

                        {
                        label:"用户名称：",
                        value:"",
                        tips:""
                    },
                    
                    {
                        label:"用户地址：",
                        value:"",
                        tips:""
                    },
                    
                    {
                        label:"联系人：",
                        value:"",
                        tips:""
                    },
                    {
                        label:"座机号码：",
                        value:"",
                        tips:""
                    },
                        {
                        label:"手机号码：",
                        value:"",
                        tips:""
                    },

                    
                ],
                desc:"",
                userNume:"",
                descoptions:[
                    {
                        name:'居民身份证',
                        value:'0'
                    },
                    {
                        name:'护照',
                        value:'1'
                    },
                    {
                        name:'户口本',
                        value:'2'
                    },
                    {
                        name:'回乡证',
                        value:'3'
                    },
                    {
                        name:'台胞证',
                        value:'4'
                    },
                ],
                userNum:"",
                PriceStrategy:"",
                isfines:"",
                UpdateCodeCopye:"",
                Remarks:"",
                year:""
            },
            histroyData:{
                
            },
        }
    },
    mounted() {
        this.init();
    },
    methods: {
        checkId(val){
            if(val ==1){
                this.idCardCheck = 1
            }else{
                this.idCardCheck = 2
            }

        },
        init() {
                this.histroyData = {
                    list: [
                        {
                        name: "张三",
                        type: "身份证",
                        num: "444210199909100030092",
                        connect: "1",
                        des: "",   
                        },
                        {
                        name: "张三",
                        type: "身份证",
                        num: "444210199909100030092",
                        connect: "0",
                        des: "",   
                        },
                    ],
                    nextPage: 0,
                    pageNum: 1,
                    pageSize: 20,
                    pages: 1,
                    prePage: 0,
                    size: 5,
                    startRow: 1,
                    total: 5,
                }

        },
        
        addTable(){
            this.histroyData.list.push({});
        },
            
        indexMethod(index) {//获取表格序号
            return (this.histroyData.pageSize - 20) * this.histroyData.pages + (index + 1);
        },

    },
    watch:{
        idCardCheck(val,oldVal){
            if(val==2){
                this.$message.error('身份证号已重复');
            }
        },
    },
}
</script>
<style lang="scss">
.wPopulation {
}
</style>