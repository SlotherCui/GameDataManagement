<template>
  <div class="backgroud">
    <div class="login">
      <div class="title">请输入管理员密码</div>
      <div class="input-group">
        <span class="iconfont search">&#xe632;</span>
        <input class="input-border"
               type="password"
               placeholder="请输入密码"
               value="password"
               show-password = 'true'
               @keydown.enter="handlePassWord"
               v-model="password"
              />
        <!--<el-button style="margin-top: 25px" round>登录</el-button>-->
        <!--<el-button style="margin-top: 25px" type="primary">登录</el-button>-->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      password: ''
    }
  },
  methods: {
    handlePassWord () {
      console.log(this.password)
      let password = {'password': this.password}
      this.axios.post('http://localhost:8080/login', password).then(this.LoginSucc)
    },
    LoginSucc (res) {
      if (res.data.code === 0) {
        this.$router.push({path: '/Home'})
      } else {
        this.$notify.error({
          title: '错误',
          message: '口令输入错误'
        })
      }
    }
  }
}
</script>

<style lang="stylus" scoped>
.backgroud
  text-align center
  position:absolute;
  width 100%
  height 100%
  background: #0cebeb;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #29ffc6, #20e3b2, #0cebeb);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #29ffc6, #20e3b2, #0cebeb); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26
  +, Opera 12+, Safari 7+ */
  .login
    margin:0 auto
    .title
      font-size: 26px
      padding-top: 24px
      font-weight: 400
      margin-bottom: 20px
      margin-top: 320px
      color #ffffff
    .input-group
      display: inline-block;
      width: 350px;
      height: 50px;
      border-radius: 50px;
      background-color: #4bd7c1;
      position relative
      .input-border
        width: 300px;
        height: 36px;
        margin-top: 6px;
        border-radius: 50px;
        color: #535353;
        font-family: sans-serif;
        background-color: #f6f6f6;
        padding-left 36px
        font-size 15px
        border: 1px solid hsla(0,0%,100%,.23);
      .search
        font-size: 20px;
        color: #4bd7c1
        position absolute
        top 50%
        margin-left 12px
        transform: translateY(-50%);
</style>
