<script>
// import router from '@/router'
import LoginForm from './LoginForm'
import ForgotForm from './ForgotForm'

export default {
  mounted () {
    this.state = 'login'
  },
  components: {
    LoginForm,
    ForgotForm
  },
  data () {
    return {
      state: undefined
    }
  },
  methods: {
    setState (name) {
      this.state = name
    }
  },
  created () {
  }
}
</script>
<template>
    <div class="page" id="login">
        <login-form id="auth-form" v-if="state === 'login'" :setState="setState"></login-form>
         <forgot-form id="auth-form" v-if="state === 'forgot'" :setState="setState"></forgot-form>
    </div>
</template>

<style lang="scss">
#login {
  margin-top: -48px;
  height:calc(100vh + 48px);
}
</style>
