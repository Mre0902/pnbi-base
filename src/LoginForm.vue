<script>
import Auth from './Auth'
import router from '@/router'

import BI_BASE_CONFIG from '@/pnbi.base.config.js'
export default {
  props: {
    setState: Function
  },
  mounted () {},
  data () {
    return {
      title: BI_BASE_CONFIG.TITLE,
      rules: {
        valid: false,
        nameRules: [
          v => !!v || 'Pflichtfeld',
          v =>
            v.length >= 3 || 'Mindestens 3 Zeichen eingeben.',
          v =>
            !this.errorAuth ||
            'Bitte überprüfen Sie ihren Namen und das Passwort.'
        ]
      },
      user: {
        username: '',
        password: ''
      },
      errorAuth: false
    }
  },

  methods: {
    focus () {
      this.errorAuth = false
      this.$refs.form.validate()
    },
    toForgot () {
      this.setState('forgot')
    },
    onSubmit ($event) {
      $event.preventDefault()
      this.errorAuth = false
      Auth.login(this.user).then(
        () => {
          router.push(BI_BASE_CONFIG.MAIN_ROUTE)
        },
        error => {
          if (error.status === 401) {
            this.errorAuth = true
            this.$refs.form.validate()
          }
        }
      )
    }
  },
  computed: {}
}
</script>
<template>
  <v-container fill-height>
    <v-layout align-center>
      <v-flex class="text-center">
        <h1 class="bi-powered">POWERED BY CORE</h1>
        <v-form v-model="rules.valid" ref="form" lazy-validation>
          <v-card>
            <v-card-title class="justify-center pb-0">
              <h1 class="bi-headline">{{title}}</h1>
            </v-card-title>  
            <v-card-text class="pb-0">      
              <v-form>          
                <v-text-field @focus="focus" label="Nutzername" v-model="user.username" :rules="rules.nameRules" required></v-text-field>
                <v-text-field @focus="focus" label="Passwort" v-model="user.password" :rules="rules.nameRules" required type="password"></v-text-field>
                <v-btn class="mb-2" color="primary" block @click="onSubmit" :disabled="!rules.valid" type="submit">Login</v-btn>
              </v-form>
            </v-card-text>   
            <v-card-actions class="white pt-0">
              <v-layout justify-center="" column>             
                <v-btn @click="toForgot" flat block>Passwort zurücksetzen</v-btn>
              </v-layout>
            </v-card-actions>
          </v-card>
        </v-form>
      </v-flex>
    </v-layout>
  </v-container>

</template>
<style lang="css">

</style>