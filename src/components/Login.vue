<template>
   <div class="login_bg">
       <div class="login_box">
           <!-- 头像 -->
           <div class="login_img">
               <img src="../assets/logo.png" alt="">
           </div>
         <!-- 登录表单区 -->
      <el-form ref="loginRef" label-width="0px" class="formBox" :model="dataForm" :rules='loginRules'>
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input
            prefix-icon="el-icon-user-solid" v-model="dataForm.username"
          ></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input prefix-icon="el-icon-lock" v-model="dataForm.password" type="password"></el-input>
        </el-form-item>
        <!-- 登录按钮 -->
        <el-form-item class="btn1">
          <el-button type="primary" @click="btnLogin">登录</el-button>
          <el-button type="info" @click="btnRes">重置</el-button>
        </el-form-item>
      </el-form>
       </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      dataForm: {
        username: 'admin',
        password: '123456'
      },
      loginRules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入用密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    //   重置
    btnRes: function () {
      this.$refs.loginRef.resetFields()
    },
    btnLogin: function () {
      this.$refs.loginRef.validate(async (data) => {
        if (!data) return this.$message.error('登录失败')
        const { data: res } = await this.$http.post('login', this.dataForm)
        console.log(res)
        if (res.meta.status !== 200) return this.$message.error('登录请求失败')
        this.$message.success('登录请求成功')
        window.sessionStorage.token = res.data.token
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_bg{
    background-color: #2b4b6b;
    height: 100%;
    .login_box{
        width: 450px;
        height: 300px;
        background-color: #fff;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
        .login_img{
            width: 100px;
            height: 100px;
            border: 1px solid #cccc;
            box-shadow: 0 .5px 3px gray;
            border-radius: 50%;
              padding: 10px;
              position: absolute;
              left: 50%;
              top: -45px;
              background-color: #fff;
              transform: translateX(-50%);
            img{
                width: 100%;
                height: 100%;
                box-sizing: border-box;
                border-radius: 50%;
                border: 1px solid #cccc;
                box-shadow: 0 0.5px 3px gray;
            }
        }
        .formBox{
            position: absolute;
            top: 85px;
            width: 100%;
            padding: 15px;
            box-sizing: border-box;
            .btn1{
                display: flex;
                justify-content: flex-end;
            }
        }
    }
}

</style>
