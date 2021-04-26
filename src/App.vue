<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark >

      <v-img
        alt="Vuetify Logo"
        class="shrink mr-2"
        contain
        src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
        transition="scale-transition"
        width="40"
      />

      <v-app-bar-title>Example application</v-app-bar-title>


    </v-app-bar>

    <v-main>
      <v-container
        fluid>
        <AuthenticationWall
          :options="options"
          @user="get_user($event)">
          <router-view/>
        </AuthenticationWall>
      </v-container>
    </v-main>

  </v-app>
</template>

<script>
import AuthenticationWall from '@/AuthenticationWall.vue'
export default {
  name: 'App',

  components: {
    AuthenticationWall
  },

  data: () => ({
    options: {
      title: 'Application wall example',
      hint: 'Username: banana, Password: banana',
      login_url: 'https://api.authentication.maximemoreillon.com/login',
      identification_url: 'https://api.authentication.maximemoreillon.com/whoami',
    }
  }),

  methods: {
    get_user(user) {
      console.log(user)
    }
  }

};
</script>
