<template>
  <div>
      <div style="display: flex;justify-content: center;margin-top: 150px">
        <el-card style="width: 350px">
          <div slot="header" class="clearfix">
            <span>登录</span>
          </div>
          <table>
            <tr>
              <td>用户名</td>
              <td>
                <el-input v-model="Manager.managerName" placeholder="请输入用户名"></el-input>
              </td>
            </tr>
            <tr>
              <td>密码</td>
              <td>
                <el-input type="password" v-model="Manager.managerPwd" placeholder="请输入密码" @keydown.enter.native="doLogin"></el-input>
                <!-- @keydown.enter.native="doLogin"当按下enter键的时候也会执行doLogin方法-->
              </td>
            </tr>
            <tr>
              <!-- 占两列-->
              <td colspan="2">
                <el-button style="width: 300px" type="primary" @click="doLogin">登录</el-button>
              </td>
            </tr>
            <el-button type="text" @click="goClient" >用户端</el-button>

          </table>

        </el-card>
      </div>
    </div>
</template>

<script>
  export default {
    name: 'Login',
    data() {
      return{
        Manager:{
          managerName:'',
          managerPwd:'',
        },

      }
    },

    methods:{

      doLogin(){
    // 	this.$axios.post("/mana/login",this.Manager)
    // 		.then(res=>{
  // 				console.log(res);//test
  // 				this.$router.push({name: 'Main'})
  // 				if(res.data==='true'){
  // 					this.$message({
  // 						message: '登陆成功',
  // 						type: 'success',
  // 						center: true
  // 					});

        this.$router.push({name: 'Main'})

// 			}

      // 				else if(res.data==='false'){
      // 					this.$message({
      // 						message: '密码错误',
      // 						type: 'error',
      // 						center: true
      // 					});
      // 				}
      // 		})
      },
      goClient(){
        this.$router.push({name: 'Client'})
      },




    }


  }

</script>
