<script>
/* eslint-disable--  */
import router from '@/router'
import Auth from './Auth'

export default {
  mounted () {},
  components: {},
  data () {
    return {
      strength: 0,
      user: {
        password: '',
        password2: ''
      },
      rules: {
        valid: false,
        passwordRules: [
          v => !!v || 'Passwort ist ein Pflichtfeld',
          v => v.length >= 6 || 'Passwort muss mindestens 6 Zeichen lang sein'
        ],
        passwordRules2: [
          v => !!v || 'Passwort ist ein Pflichtfeld',
          v => v.length >= 6 || 'Passwort muss mindestens 6 Zeichen lang sein',
          v => this.isValid || 'Passwörter sind nicht identisch'
        ]
      }
    }
  },
  methods: {
    toLogin () {
      router.go('login')
    },
    focus () {
      this.errorAuth = false
      this.$refs.form.validate()
    },
    goToLogin () {
      router.push('/')
    },
    onSubmit ($event) {
      $event.preventDefault()
      Auth.reset({
        password: this.user.password,
        password_code: this.$route.params.code
      })
        .then(response => {
          console.log(response)
        })
        .catch(err => {
          console.log(err)
        })
    },
    onChange (data) {
      if (this.user.password.length > 0) {
        Auth.password({
          password: this.user.password
        })
          .then(response => {
            this.strength = Math.round(response.strength * 100)
          })
          .catch(err => {
            console.log(err)
          })
      }
    }
  },
  computed: {
    isValid: {
      get: function () {
        const valid = this.user.password === this.user.password2
        if (this.user.password.length !== 0) {
          return valid
        } else {
          this.strength = 0
          return !valid
        }
      }
    },
    progress () {
      if (this.strength < 20) {
        this.strength = 0
        return this.strength
      } else {
        return this.strength
      }
    },
    color () {
      if (this.strength < 33) {
        return ['error', 'warning', 'success'][0]
      } else if (this.strength > 33 && this.strength < 66) {
        return ['error', 'warning', 'success'][1]
      } else if (this.strength > 66) {
        return ['error', 'warning', 'success'][2]
      }

      /* if (true){
        return ["error", "warning", "success"][Math.floor(this.progress / 40)];
      }else {
        return ["error", "warning", "success"][2]
      }; */
    }
  },
  created () {}
}
</script>

<template>
  <div class="page" id="login">
    <v-container class="page" id="auth-form" fluid="" fill-height>
      <v-layout align-center>
        <v-flex class="text-center">
          <h1 class="bi-powered">POWERED BY CORE</h1>
          <v-form ref="form" v-model="rules.valid"> 
            <v-card >
              <v-card-title >
                <h2 >Neues Passwort</h2>
              </v-card-title>
              <v-card-text class="py-0" >              
                <v-text-field type="password" @focus="focus" label="Neues Passwort" v-model="user.password" :rules="rules.passwordRules" @input="onChange" required></v-text-field>
                <v-card-text class="pt-0 pb-0 progress-bar">
                  <span>Passwortstärke:</span>
                  <v-progress-linear slot="progress" :value="progress" height="5" :color="color"></v-progress-linear>
                </v-card-text>
                <v-text-field type="password" @focus="focus" label="Passwort wiederholen" v-model="user.password2" :rules="rules.passwordRules2" required></v-text-field>
                <v-btn block color="primary" type="submit" :disabled="!rules.valid" @click="onSubmit">Neues Passwort setzen</v-btn>
                <v-btn @click="goToLogin" flat block>Zurück zum Login</v-btn>
              </v-card-text>
            </v-card>
          </v-form>   
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<style lang="scss">

</style>
