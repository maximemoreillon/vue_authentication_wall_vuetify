<template>
  <v-form
    class="text-center"
    @submit.prevent="login()">

    <v-text-field
      label="Username"
      v-model="username"/>

    <v-text-field
      label="Password"
      type="Password"
      v-model="password"/>

    <v-btn
      dark
      type="submit"
      @click="login"
      :loading="processing">
      <v-icon>mdi-login</v-icon>
      <span>Login</span>
    </v-btn>

    <v-snackbar
      color="#C00000"
      dark
      v-model="snackbar.show" >
      {{ snackbar.text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          icon
          v-bind="attrs"
          @click="snackbar.show = false" >
          <v-icon>mdi-close</v-icon>
        </v-btn>
      </template>
    </v-snackbar>

  </v-form>
</template>

<script>
/*
This component exchanges credentials for a JWT and manages the storage of the JWT in cookies
*/

import axios from 'axios'
import VueCookies from 'vue-cookies'
//import StoreMixin from '../mixins/store.js'

export default {
  name: 'LoginForm',
  props: {
    options: Object,
  },
  //mixins: [StoreMixin],
  components: {

  },
  data(){
    return {
      username: '',
      password: '',
      snackbar: {
        show: false,
        text: ''
      },
      processing: false,
    }
  },
  methods: {
    login(){
      // Send credentials and get JWT

      const url = this.options.login_url
      const body = { username: this.username, password: this.password }

      this.snackbar.show = null
      this.processing = true

      axios.post(url, body)
      .then( ({data}) => {
        if(!data.jwt) return
        VueCookies.set('jwt', data.jwt)

        // clear the inputs
        this.username = ''
        this.password = ''

        this.$emit('loggedIn')
      })
      .catch( (error) => {
        if(error.response) this.snackbar.text = error.response.data
        else this.snackbar.text = `Error while logging in`
        this.snackbar.show = true
        console.error(error)
       })
      .finally(() => {

        this.processing = false

      })

    },

  },

}
</script>

<style scoped>


</style>
