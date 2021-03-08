<template>
  <Form ref="loginForm" :model="form" :rules="rules" @keydown.enter.native="handleSubmit">
    <FormItem prop="userName">
      <Input v-model="form.userName" placeholder="请输入用户名">
        <span slot="prepend">
          <Icon :size="16" type="ios-person"></Icon>
        </span>
      </Input>
    </FormItem>
    <FormItem prop="password">
      <Input type="password" v-model="form.password" placeholder="请输入密码">
        <span slot="prepend">
          <Icon :size="14" type="md-lock"></Icon>
        </span>
      </Input>
    </FormItem>
    <FormItem prop="verifyCode" v-if="verifyCodeEnable">
      <div style="display: flex;align-items: center;">
        <Input v-model="form.verifyCode" placeholder="请输入验证码">
          <span slot="prepend">
            <Icon :size="14" type="ios-keypad-outline"></Icon>
          </span>
        </Input>
        <div style="width:150px; margin-left:10px" @click="handleVerifyCodeSrc">
          <img :src="verifyCodeSrc" style="height:33px; width: 100px; cursor: pointer;" alt="点击更换" title="点击更换" />
        </div>
      </div>
    </FormItem>
    <FormItem>
      <Button @click="handleSubmit" type="primary" long>登录</Button>
    </FormItem>
  </Form>
</template>
<script>
export default {
  name: 'LoginForm',
  props: {
    verifyCodeEnable: {
      type: Boolean,
      default: false
    },
    verifyCodeSrc: {
      type: String,
      default: ''
    },
    userNameRules: {
      type: Array,
      default: () => {
        return [
          { required: true, message: '账号不能为空', trigger: 'blur' }
        ]
      }
    },
    passwordRules: {
      type: Array,
      default: () => {
        return [
          { required: true, message: '密码不能为空', trigger: 'blur' }
        ]
      }
    },
    verifyCodeRules: {
      type: Array,
      default: () => {
        return [
          { required: true, message: '验证码不能为空', trigger: 'blur' }
        ]
      }
    }
  },
  data () {
    return {
      form: {
        userName: '',
        password: '',
        verifyCode: ''
      }
    }
  },
  computed: {
    rules () {
      return {
        userName: this.userNameRules,
        password: this.passwordRules,
        verifyCode: this.verifyCodeRules
      }
    }
  },
  methods: {
    handleVerifyCodeSrc () {
      this.$emit('on-get-verify-code', {})
    },
    handleSubmit () {
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          this.$emit('on-success-valid', {
            userName: this.form.userName,
            password: this.form.password,
            verifyCode: this.form.verifyCode
          })
        }
      })
    }
  }
}
</script>
