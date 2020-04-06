<template>
  <div class="login-container">
    <div class="login-box">
      <div class="avatar">
        <img src="../assets/logo.png" />
      </div>
      <el-form class="login-form" :model="form" :rules="rule" ref="loginFormRef">
        <el-form-item prop="username">
          <el-input
            v-model="form.username"
            prefix-icon="el-icon-user-solid"
            autocomplete="off">
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            v-model="form.password"
            prefix-icon="el-icon-lock"
            type="password"
            autocomplete="off">
          </el-input>
        </el-form-item>
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button @click="resetLoginForm('loginFormRef')">重置</el-button>
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
      form: {
        username: 'admin',
        password: '123456'
      },
      //  表单验证规则
      rule: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
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
    login () {
      this.$refs.loginFormRef.validate(async (valid) => {
        if (valid) {
          const {data: res} = await this.$http.post('login', this.form)
          if (+res.meta.status === 200) {
            this.$message({
              message: '登录成功',
              type: 'success'
            })
            // 将token存储在sessionstora中，在当前会话生效
            window.sessionStorage.setItem('token', res.data.token)
            this.$router.push('/home')
          } else {
            this.$message({
              message: res.meta.msg,
              type: 'error'
            })
          }
        }
      })
    },
    resetLoginForm (formName) {
      this.$refs[formName].resetFields()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  lang="less" scoped>
.login-container {
  background-color: #2b4b6b;
  height: 100%;
  overflow: hidden;
}
.login-box {
  width: 450px;
  height: 300px;
  background-color: #ffffff;
  border-radius: 3px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
}
.avatar {
  width: 130px;
  height: 130px;
  border-radius: 50%;
  border: 1px solid #eeeeee;
  box-shadow: 0 0 10px #ddd;
  padding: 10px;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eeeeee;
  }
}
.login-form {
  margin-top: 100px;
  padding: 0 30px;
  .btns {
    text-align: right;
  }
}
</style>
