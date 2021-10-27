<template>
  <div class="login-container">
    <!-- 头像 -->
    <div class="login-box">
      <div class="assets_logo">
        <img src="../assets/logo.png"  alt="">
      </div>
      <!-- 输入 -->
     <el-form :model="iconFrom" class="input_box" :rules="iconFromRules" ref="loginFromRef">
      <el-form-item prop="username">
       <el-input prefix-icon="iconfont icon-user" v-model="iconFrom.username"></el-input>
      </el-form-item>
      <el-form-item prop="password">
       <el-input  prefix-icon="iconfont icon-3702mima"  type="password" v-model="iconFrom.password"></el-input>
      </el-form-item>
      <el-form-item class="btns">
       <el-button type="primary" @click="login">登录</el-button>
       <el-button type="info" @click="reset">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      // 表单登录数据绑定
      iconFrom: {
        username: 'admin',
        password: '123456'
      },
      // 表单登录验证
      iconFromRules: {
        username: [
          { required: true, message: '请输入名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 登录预验证
    login () {
      this.$refs.loginFromRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.iconFrom)
        console.log(res)
        if (res.meta.status !== 200) {
          this.$message.error('登陆失败')
        } else {
          this.$message.success('登陆成功')
          // 保存token
          window.sessionStorage.setItem('token', res.data.token)
          // 跳转到后台主页
          this.$router.push('/home')
        }
      }
      )
    },
    // 重置
    reset () {
      this.$refs.loginFromRef.resetFields()
    }
  }
}
</script>

<style lang="less" scoped>
.login-container{
  background: #2b4b6b;
  height: 100vh;
  .login-box{
    width: 450px;
    height: 300px;
    background: #ffffff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
    .assets_logo{
      height: 130px;
      width: 130px;
      background: #fff;
      border: 1px solid #eee;
      border-radius: 50%;
      box-shadow: 0 0 10px #ddd;
      padding: 10px;
      position: absolute;
      left: 50%;
      transform: translate(-50%,-50%);
      img{
        border-radius: 50%;
        width: 100%;
        height: 100%;
        background-color: #eee;
      }
    }
    .input_box{
      position: relative;
      width: 90%;
      left: 50%;
      transform: translate(-50%,65%);
      box-sizing: border-box;
    }
  }
}
    .btns{
      display: flex;
      justify-content: flex-end;
    }

</style>
