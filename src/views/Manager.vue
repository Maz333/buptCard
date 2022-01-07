<template>
  <div>

    <el-container >

      <el-header>
        <tr>

          <td style="width: 400px" ><font size=20px >Hi,{{managerList.mName}}!</font> </td>

          <td >
            <el-input  placeholder="search" prefix-icon="el-icon-search" style="width: 500px " ></el-input>
          </td>

          <td style="width: 100px" align=right><el-button @click="goMain">退出管理员界面</el-button></td>

          <el-button type="info" icon="el-icon-plus" plain @click="dialogFormVisible = true">添加管理员</el-button>
          <el-dialog title="新管理员" :visible.sync="dialogFormVisible">
            <el-form :model="form">
              <el-form-item label="学工号" :label-width="formLabelWidth">
                <el-input v-model="form.Id" autocomplete="off"></el-input>
              </el-form-item>
              <el-form-item label="权限级别" :label-width="formLabelWidth">
                <el-select v-model="form.Status" placeholder="请设置权限级别">
                  <el-option label="普通" value="0"></el-option>
                  <el-option label="高级" value="1"></el-option>
                </el-select>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="dialogFormVisible = false">取 消</el-button>
              <el-button type="primary" @click="goAddmanager(form.Id,form.Status)">确 定</el-button>
            </div>
          </el-dialog>


        </tr>
      </el-header>

      <el-main>
      <!-- 表格 -->
        <el-table
          :data="managerList"
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
            prop="mName"
            label="管理员"
            align="center"
          >
          </el-table-column>

          <el-table-column
            prop="mId"
            label="学工号"
            align="center"
          >
          </el-table-column>

          <el-table-column
            prop="mPower"
            label="权限"
            align="center"
          >
          </el-table-column>

          <el-table-column
            prop="actions"
            label="操作"
            align="center"
          >


            <template style="text-align: center; margin: 0" slot-scope="scope">


              <el-popover
                placement="top"
                title="更改权限为"
                trigger="click"
              >
                <div >
                  <el-radio v-model="radio" label="0" v-show="scope.row.mPower!=0">普通管理员</el-radio>
                  <el-radio v-model="radio" label="1" v-show="scope.row.mPower!=1">高级管理员</el-radio>
                </div>
                <div style="text-align: right; ">
                  <el-button type="primary" size="mini" @click="goChangePower(scope.row.mId,radio,scope.row.mPower)">确定</el-button>
                </div>
                <el-button slot="reference" type="warning" icon="el-icon-brush" plain>更改权限</el-button>
              </el-popover>

              <el-button  type="danger" icon="el-icon-delete" @click="goDelete(scope.row.mId)" plain>删除</el-button>








            </template>
          </el-table-column>

        </el-table>


      </el-main>

    </el-container >




  </div>
</template>

<script>
  export default {
    name: 'Manager',
    data() {
      return {
        managerList:{},
        tabPosition:'left',
        Deal:{},
        dialogFormVisible: false,
        visible: false,
        form: {
          name: '',
          region: '',
          date1: '',
          date2: '',
          delivery: false,
          type: [],
          resource: '',
          desc: ''
        },
        formLabelWidth: '80px',
        radio: '1',

      }
    },

    mounted:function(){
      this.getManager();//需要触发的函数

    },




    methods:{
      goMain(){
        this.$router.push({name: 'Main'});
      },

      getManager(){
        this.$axios.get("/allmanager")
          .then(res=>{
            console.log(res);
            this.managerList=res.data;
          })
      },

      goAddmanager(Id,Power){
        console.log(Id,Power);
        this.Deal.mId=Id;
        this.Deal.mPower=Power;
        this.$axios.post("/manager/add",this.Deal)
          .then(res=>{
            console.log(res);
            if(res.data == "Success"){
              this.$message({
                message: '添加管理员成功',
                type: 'success',
                center: true
              });
            }
            else if(res.data == "Manager exist"){
              this.$message({
                message: '管理员已存在',
                type: 'warning',
                center: true
              });
            }
            else if(res.data == "No user"){
              this.$message({
                message: '查无此人',
                type: 'warning',
                center: true
              });
            }
            else{
              this.$message({
                message: '添加管理员失败',
                type: 'error',
                center: true
              });
            }

          });
        this.getManager();

      },

      goDelete(Id){
        this.Deal.mId=Id;
        this.$confirm('此操作将永久删除该管理员, 是否继续?', '删除', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$axios.post("/manager/delete",this.Deal)
          .then(res=>{
            console.log(res);
            if(res.data == "Success"){
              this.$message({
                message: '删除成功',
                type: 'success',
                center: true
              });
            }
            else{
              this.$message({
                message: '删除失败',
                type: 'error',
                center: true
              });
            }
            this.getManager();

          });
        }).catch(() => {
          this.$message({
            type: 'info',
            center: true,
            message: '已取消删除'
          });
        });


      },
      goChangePower(Id,newPower,currentPower){
        this.Deal.mId=Id;
        this.Deal.mPower=newPower;
        console.log(this.Deal);
        if(newPower==currentPower){
          this.$message({
            message: '当前权限未变动，请点击设置后确定',
            type: 'warning',
            center: true
          });
        }
        else{
          this.$axios.post("/manager/changePower",this.Deal)
          .then(res=>{
            console.log(res);
            if(res.data == 'Success'){
              this.$message({
                message: '修改成功',
                type: 'success',
                center: true
              });
            }
            else{
              this.$message({
                message: '修改失败',
                type: 'error',
                center: true
              });
            }
          });

        }
        this.getManager();
      },









	},




  }
</script>

<style>
</style>
