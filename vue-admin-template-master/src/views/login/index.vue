<template>
  <div class="login-container">
    <!-- 左右图 -->
    <div class="bannerbox">
      <img src="./bgi-ov1.webp" alt style="position: absolute;left:0;top:0;width:100%,height:100%" />
      <img src="./bgi-rec.jpg" alt style="position: absolute;right:0;top:0;width:100%,height:100%" />
    </div>
    <el-form
      ref="loginForm"
      :model="loginForm"
      :rules="loginRules"
      class="login-form"
      auto-complete="on"
      label-position="left"
    >
      <!-- 表头 -->
      <div class="title-container">
        <h3 class="title">
          <img src="./rec-blue.jpg" alt />
        </h3>
        <div class="title-two">
          <img src="./rec-purple.jpg" alt />
        </div>
      </div>
      <!-- 标题 -->
      <div class="header" style="width:100px;height:150px;">
        <img src="./zhadmin.jpg" alt />
        <!-- <img src="./WELCOME.jpg" alt=""> -->
        <!-- <img src="./foryour.jpg" alt=""> -->
      </div>
      <div
        style="width:50%;height:20px;line-height:20px;font-size:18px;font-weight:600;position:absolute;top:135px"
      >WELCOME! Admin !</div>
      <div
        style="width:50%;height:20px;line-height:20px;font-size:12px;font-weight:100;position:absolute;top:195px"
      >为你的安全保驾护航！</div>
      <div class="right-img">
        <img
          src="./drawbg.webp"
          alt
          style="width:100%;height:100%;position:absolute;right:10%;top:32%;width:35%;height:60%"
        />
        <img
          src="./draw-main.webp"
          alt
          style="position:absolute;right:10%;top:22%;width:30%;height:60%"
        />
        <img
          src="./rec-yellow.jpg"
          alt
          style="position: absolute;right:0;bottom:0;width:100%,height:100%"
        />
      </div>
      <!-- 用户名 -->
      <el-form-item prop="username" style="width:25%;border-radius:20px">
        <span class="svg-container">
          <svg-icon icon-class="user" />
        </span>
        <el-input
          ref="username"
          v-model="loginForm.username"
          placeholder="请输入用户名"
          name="username"
          type="text"
          tabindex="1"
          auto-complete="on"
        />
      </el-form-item>
      <!-- 密码 -->
      <el-form-item prop="password" style="width:25%;border-radius:20px">
        <span class="svg-container">
          <svg-icon icon-class="password" />
        </span>
        <el-input
          :key="passwordType"
          ref="password"
          v-model="loginForm.password"
          :type="passwordType"
          placeholder="请输入密码"
          name="password"
          tabindex="2"
          auto-complete="on"
          @keyup.enter.native="handleLogin"
        />

        <span class="show-pwd" @click="showPwd">
          <svg-icon :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
        </span>
      </el-form-item>
      <!-- 忘记密码 -->
      <img src="./忘记密码.webp" alt="" style="position:absolute;top:390px;left:220px">
      <!-- 登录 -->
      <el-button
        :loading="loading"
        type="primary"
        style="width:25%;margin-bottom:30px;border-radius:20px"
        @click.native.prevent="handleLogin"
      >
        <img src="./登录@2x.webp" alt />
      </el-button>

      <div class="tips">
        <span style="margin-right:20px;">username: admin</span>
        <span>password: any</span>
      </div>
    </el-form>
    <!-- 其余小图标 -->
    <img src="./bgi-ov2.webp" alt="" style="position:absolute;    top: 11%;
    left: 36%">
    <img src="./oval-blue1.webp" alt="" style="position:absolute;bottom: 15%;
    left: 8%;">
    <img src="./oval-purple10.webp" alt="" style="position:absolute;    bottom: 2%;
    left: 50%;">
    <img src="./bgi-ov.webp" alt="" style="position:absolute;    bottom: 5%;
    left: 20%;">
  </div>
</template>

<script>
import { validUsername } from '@/utils/validate'

export default {
  name: 'Login',
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!validUsername(value)) {
        callback(new Error('请输入正确的账号'))
      } else {
        callback()
      }
    }
    const validatePassword = (rule, value, callback) => {
      // var str = /^[a-z][a-z]+\d+$/i
      // if (value.length < 6 && str.test(value)){
        console.log(value+"qqq")
      if (value.length < 6 && /^[a-z][a-z]+\d+$/i.test(this.loginForm.password)){

        console.log(value)
        console.log(value.length)
       
        // console.log(str.test(value))
        // console.log(str.test(value).length)
        callback(new Error('请输入6位以上的数字字母组合1'))
      } 
      else {
        callback()
      }
      

   

}  
      
      
    
    return {
      loginForm: {
        username: '',
        password: ''
      },
      loginRules: {
        username: [{ required: true, trigger: 'blur', validator: validateUsername }],
        password: [{ required: true, trigger: 'blur', validator: validatePassword }]
      },
      loading: false,
      passwordType: 'password',
      redirect: undefined
    }
  },
  watch: {
    $route: {
      handler: function(route) {
        this.redirect = route.query && route.query.redirect
      },
      immediate: true
    }
  },
  methods: {
    showPwd() {
      if (this.passwordType === 'password') {
        this.passwordType = ''
      } else {
        this.passwordType = 'password'
        console.log(this.passwordType)
      }

      this.$nextTick(() => {
        this.$refs.password.focus()
      })
      
    },
  handleLogin() {

    if (this.loginForm.username && this.loginForm.password){
      if (/^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{6,16}$/.test(this.loginForm.password)){
      this. loading = true;
      this. $store.dispatch("user/login", this. loginForm).then(() => {
      this. $router . push({ path: this. redirectll || "/" });this.loading = false;
      }).catch(() => {
      this.loading = false;
    });
      } else {
          alert('请输入6位以上的数字字母组合')
            return false;
        }
        }else {
          alert('请输入正确的用户名')
            return false;
          }
  }
},
}

</script>

<style lang="scss">
/* 修复input 背景不协调 和光标变色 */
/* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */

$bg: #283443;
$light_gray: #fff;
$cursor: #fff;

@supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
  .login-container .el-input input {
    color: $cursor;
  }
}

/* reset element-ui css */
.login-container {
  
  .el-input {
    display: inline-block;
    height: 47px;
    width: 85%;

    input {
      background: transparent;
      border: 0px;
      -webkit-appearance: none;
      border-radius: 0px;
      padding: 12px 5px 12px 15px;
      color: $light_gray;
      height: 47px;
      caret-color: $cursor;

      &:-webkit-autofill {
        box-shadow: 0 0 0px 1000px $bg inset !important;
        -webkit-text-fill-color: $cursor !important;
      }
    }
  }

  .el-form-item {
    border: 1px solid rgba(255, 255, 255, 0.1);
    background: rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    color: #454545;
  }
}
</style>

<style lang="scss" scoped>
$bg: #2d3a4b;
$dark_gray: #889aa4;
$light_gray: #eee;

.login-container {
  min-height: 100%;
  width: 100%;
  // background-color: $bg;
  background-image: url("./bgi-ov1.jpg");
  background-repeat: no-repeat;
  background-size: 200% 200%;
  overflow: hidden;
  position: relative;
  .login-form {
    position: relative;
    width: 80%;
    height: 80%;
    max-width: 100%;
    padding: 100px 35px 0;
    margin: 0 auto;
    overflow: hidden;
    background-color: #fff;
    // margin:100px auto;
    top: 50%;
    height: 100%;
    margin-top: 100px;
    // left: 13%;
    // margin-left: -120px;
  }

  .tips {
    font-size: 14px;
    color: #fff;
    margin-bottom: 10px;

    span {
      &:first-of-type {
        margin-right: 16px;
      }
    }
  }

  .svg-container {
    padding: 6px 5px 6px 15px;
    color: $dark_gray;
    vertical-align: middle;
    width: 30px;
    display: inline-block;
  }

  .title-container {
    h3 {
      position: absolute;
      top: 0;
      left: 0;
    }

    .title {
      font-size: 26px;
      color: $light_gray;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }
    .title-two {
      position: absolute;
      top: 0;
      left: 155px;
      width: 10%;
      height: 7%;
      img {
        width: 100%;
        height: 100%;
      }
    }
    .header {
      // position: absolute;
      // top:0;
      margin-top: 10px;
    }
  }

  .show-pwd {
    position: absolute;
    right: 10px;
    top: 7px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
    user-select: none;
  }
}
</style>
