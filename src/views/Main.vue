<template>
	<div>


    <el-container >


      <el-header>
        <tr>

          <td style="width: 400px" ><font size=20px >Hi,{{managerList.mName}}!</font> </td>

          <td >
            <el-input  placeholder="search" prefix-icon="el-icon-search" style="width: 500px " ></el-input>
          </td>

          <td style="width: 100px" align=right><el-button @click="logout">退出登录</el-button></td>

          <td style="width: 100px" align=right><el-button type="info" plain @click="goManager" >管理员界面  v-if="managerList.mPower==='admin'"</el-button></td>

        </tr>
      </el-header>

      <el-main>
        <el-tabs :tab-position="tabPosition" >
          <el-tab-pane label="待补卡">
          <!-- 表格 -->
            <el-table
              :data="applyList"
              border

              style="width: 100%"

              >
              <el-table-column
                type="index"
                label="序号"
                width="80px"
                align="center">
              </el-table-column>

              <el-table-column
                prop="dealId"
                label="编号"
                width="80px"
                align="center">
              </el-table-column>

              <el-table-column
                prop="applyName"
                label="申请人"
                align="center"
              >
              </el-table-column>

              <el-table-column
                prop="applyDate"
                label="申请日期"
                align="center"
              >
              </el-table-column>

              <el-table-column
                prop="dealStatus"
                label="支付状态"
                align="center">

              </el-table-column>

              <el-table-column
                prop="actions"
                label="补卡"
                align="center"

              >
                <template slot-scope="scope">
                   <el-button type="info" icon="el-icon-plus" circle @click="finishMake(scope.row.dealId)" v-if="scope.row.dealStatus==1"></el-button>
                   <el-button type="info" icon="el-icon-plus" circle  v-if="scope.row.dealStatus==0" plain disabled></el-button>
                </template>
              </el-table-column>

            </el-table>
          </el-tab-pane>

          <el-tab-pane label="待取卡">
          <!-- 表格 -->
            <el-table
              :data="applyList"
              border

              style="width: 100%"

              >
              <el-table-column
                type="index"
                label="序号"

                align="center">
              </el-table-column>

              <el-table-column
                prop="applyName"
                label="申请人"

                align="center"
              >
              </el-table-column>

              <el-table-column
                prop="applyDate"
                label="申请日期"

                align="center"
              >
              </el-table-column>

              <el-table-column
                prop="getDate"
                label="取卡日期"
                align="center"
              >
              </el-table-column>

              <el-table-column
                prop="dealStatus"
                label="状态"
                align="center"
              >
              </el-table-column>

              <el-table-column
                prop="actions"
                label="取卡"
                align="center"

              >
                <template slot-scope="scope">
                   <el-button type="info" icon="el-icon-plus" circle @click="finishDeal(scope.row.dealId)" v-if="scope.row.dealStatus==2"></el-button>
                </template>
              </el-table-column>

            </el-table>

          </el-tab-pane>

          <el-tab-pane label="已完成">
          <!-- 表格 -->
            <el-table
              :data="applyList"
              border

              style="width: 100%"

              >
              <el-table-column
                type="index"
                label="序号"

                align="center">
              </el-table-column>

              <el-table-column
                prop="applyName"
                label="申请人"

                align="center"
              >
              </el-table-column>

              <el-table-column
                prop="applyDate"
                label="申请日期"
                align="center"
              >
              </el-table-column>

              <el-table-column
                prop="getDate"
                label="取卡日期"
                align="center"
              >
              </el-table-column>





            </el-table>
          </el-tab-pane>

          <el-tab-pane label="全部">全部</el-tab-pane>
        </el-tabs>
      </el-main>
    </el-container>
  </div>
</template>


<script>
import axios from 'axios';
import qs from 'qs';
  export default {
    name: 'Main',
    data(){
      return{

        Deal:{
          dealId:0,
          dealStatus:0
        },
        applyList:{
          status:''
        },

        search:'',
        tabPosition: 'left',
        dialogFormVisible :false,
        formLabelWidth : '120px',

        customColor: '#409eff',
        customColors: [
          {color: '#43d400', percentage: 50},
          {color: '#f0cc00', percentage: 80},
          {color: '#d51717', percentage: 100}
        ],

        dealList:{
          stuId:'2019212358'
        },
        dealList:[],
        managerList:{
          mPower:"admin"
        },
        onpayStatus:'',
      };
    },//data
    mounted:function(){
      this.getInfo();
    },


    methods:{
      getInfo(){
        console.log("getInfo")
        this.$axios.get("/alluser")
          .then(res=>{
            console.log(res);
            this.userList=res.data;
          })
        this.$axios.get("/alldeal")
          .then(res=>{
            console.log(res);
            this.applyList=res.data;

          })
        this.$axios.get("/allmanager")
          .then(res=>{
            console.log(res);
            this.managerList=res.data;
          })

      },
  /*    onpay(status){
        if(status===0){
          onpayStatus :'未支付'
        }
        else{
          onpayStatus:'已支付'
        }
      }, */

      finishMake(Id){
        this.Deal.dealId=Id;
        this.Deal.dealStatus=2;
        console.log(this.Deal);
        this.$axios.post("/deal/update", this.Deal)
          .then(res=>{
            console.log(res);

            if(res.data == "Success"){
              this.$message({
                message: '补卡成功',
                type: 'success',
                center: true
              });
            }
            else {
              this.$message({
                message: '补卡失败',
                type: 'error',
                center: true
              });
            }
            this.getInfo();
          })
      },

      finishDeal(Id){
        this.Deal.dealId=Id;
        this.Deal.dealStatus=3;
        console.log(this.Deal);
        this.$axios.post("/deal/update", this.Deal)
          .then(res=>{
            console.log(res);

            if(res.data == "Success"){
              this.$message({
                message: '补卡成功',
                type: 'success',
                center: true
              });
            }
            else if(res.data == "DateError"){
              this.$message({
                message: '获取时间失败，补卡成功',
                type: 'warning',
                center: true
              });
            }
            else {
              this.$message({
                message: '补卡失败',
                type: 'error',
                center: true
              });
            }
            this.getInfo();
          })
      },

      goManager(){
       console.log("goManager");
        this.$router.push({name: 'Manager'});
      },

      logout(){


        this.$confirm('？？？', '确定退出登录吗',
          {confirmButtonText: '确定',})
          .then(() =>{

            this.$axios.get("/manager/logout")
              .then(res=>{
                  if(res.data==='Logout'){
                    this.$message({
                      message: '退出成功',
                      type: 'success',
                      center: true
                    });
                    this.$router.push({name: 'Login'})
                  }
              })

        })
      },



    },//methods




    dosearch(){
      alert(JSON.stringify(this.search))
    },


    customColorMethod(percentage) {
      if (percentage < 30) {
        return '#909399';
      }
      else if (percentage < 70) {
        return '#e6a23c';
      }
      else {
        return '#67c23a';
      }
    },



}
</script>

<style>
  .el-table__header{ width: 100% !important; } .el-table__body{ width: 100% !important; }
</style>
