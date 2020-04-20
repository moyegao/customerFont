
<template>
    <div class="FlowingDate">
         <div class="">
             <div class="toolbar">
              <el-form :inline="true" size="mini" :model="FlowingToolbar" class="demo-form-inline">

                <el-form-item class="form-left">

                  <el-form-item label="时间：">
                    <el-date-picker
                      v-model="FlowingToolbar.time"
                      type="datetimerange"
                      range-separator="至"
                      start-placeholder="开始日期"
                      end-placeholder="结束日期">
                    </el-date-picker>
                  </el-form-item>

                  <el-form-item>

                    <el-button class='searchBtn' @click="demand" icon="el-icon-search"></el-button>
                  </el-form-item>

                </el-form-item>
              </el-form>
            </div>

            <el-table highlight-current-row stripe border :data="FlowingTableDate" class="change-tables-table">


              <el-table-column type="index" label="NO." width="50" fixed="left" :index="indexMethod">
              </el-table-column>

              <el-table-column prop="code" min-width="180" label="时间">
              </el-table-column>

              <el-table-column prop="code" min-width="180" label="项目">
              </el-table-column>

              <el-table-column prop="concentrator" min-width="100" label="金额">
              </el-table-column>

              <el-table-column prop="daytime" min-width="100" label="余额">

              </el-table-column>

            </el-table>
            <!-- <div slot="footer" class="dialog-footer">
              <el-button size="mini" @click="outerVisidialogTableVisibleble = false">取 消</el-button>
              <el-button size="mini" type="primary" @click="dialogTableVisible = false">确定切换</el-button>
            </div> -->
        </div>
    </div>
</template>
<script>
export default {
    name:"FlowingDate",
    data(){
        return{
          
          FlowingTableDate:[],
          FlowingTableVisible:true,
          FlowingToolbar:{

          },
        }
    }
}
</script>
<style lang="scss"> 
 @import "../../../assets/styles/scss/base/fn";

  $imgWidth: 200px;
$theme-color:#297acc;
.FlowingDate{
    height: 100%;
    // width: 80%;
    margin: 0 auto;
    // .backfillSubmitForm{
    //     // margin-top: 15px;
    //     // padding: 10px 20px;
    //     // // width: 93%
    // }
}
</style>
