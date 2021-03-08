<template>
  <div class="login">
    <div class="login-con">
      <Card icon="log-in" title="欢迎登录" :bordered="false">
        <div class="form-con">
          <login-form @on-success-valid="handleSubmit" @on-get-verify-code="getVerifyCodeSrc" :verifyCodeEnable="verifyCodeEnable" :verifyCodeSrc="verifyCodeSrc"></login-form>
          <!-- <p class="login-tip">输入任意用户名和密码即可</p> -->
        </div>
      </Card>
    </div>
  </div>
</template>

<script>
import LoginForm from '_c/login-form'
import { mapActions } from 'vuex'
export default {
  data () {
    return {
      baseUrl: '',
      verifyCodeEnable: false,
      verifyCodeSrc: null
    }
  },
  components: {
    LoginForm
  },
  mounted () {
    this.baseUrl = process.env.NODE_ENV === 'development' ? this.$config.baseUrl.dev : this.$config.baseUrl.pro
    this.getVerifyCodeEnable()
  },
  methods: {
    ...mapActions([
      'handleVerifyCodeEnable',
      'handleLogin',
      'getUserInfo'
    ]),
    getVerifyCodeEnable () {
      this.handleVerifyCodeEnable().then(res => {
        this.verifyCodeEnable = res.data
        this.getVerifyCodeSrc()
      })
    },
    getVerifyCodeSrc () {
      if (this.verifyCodeEnable) {
        this.verifyCodeSrc = this.baseUrl + 'verify/code?timestamp=' + new Date().getTime()
      }
    },
    handleSubmit ({ userName, password, verifyCode }) {
      this.handleLogin({ userName, password, verifyCode }).then(res => {
        this.getUserInfo().then(res => {
          this.$router.push({
            name: this.$config.homeName
          })
        })
      })
    }
  }
}
</script>

<style lang="less">
  @import './login.less';
</style>
