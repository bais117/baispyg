<template>
  <div class="login-wrap">
    <el-form class="login-form" label-position="top" label-width="80px" :model="formdata">
      <h2>用户登录</h2>
      <el-form-item label="用户名">
        <el-input v-model="formdata.username"></el-input>
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="formdata.password"></el-input>
      </el-form-item>
      <el-button @click.prevent="handleLogin()" class="login-button" type="success">登录</el-button>
    </el-form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      formdata: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    async handleLogin () {
      // ES7新特性 async await

      // 前提api-server->node app启动服务器
      const res = await this.$http.post(`login`, this.formdata)
      console.log(res)
      const {
        data: {
          data: { token },
          meta: { msg, status }
        }
      } = res
      if (status === 200) {
        localStorage.setItem('token', token)
        // console.log('success--')
        this.$router.push({
          name: 'home'
        })
      } else {
        // console.log('err---')
        this.$message.error(msg)
      }

      // .then((res)=> {
      //   console.log(res)
      //   //多层对象解构赋值
      //   const {data:{data,meta:{msg,status}}} = res
      //   if(status === 200) {
      //     // console.log('success--')
      //     this.$router.push({
      //       name:"home"
      //     })
      //   } else {
      //     // console.log('err---')
      //      this.$message.error(msg)
      //   }
      // })
    }
  }
}
</script>

<style>
.login-wrap {
  /* 注意: 百分比布局 父元素height */
  height: 100%;
  background-color: #324152;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login-form {
  background-color: #ffffff;
  border-radius: 5px;
  /* 开发 */
  width: 400px;
  padding: 30px;
}
.login-btn {
  width: 100%;
}
</style>
