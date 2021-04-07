<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

    </v-app-bar>

    <v-main>
      <AuthenticationWall
        :options="options">
        <router-view/>
      </AuthenticationWall>
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
      login_url: 'https://api.authentication.maximemoreillon.com/login',
      identification_url: 'https://api.authentication.maximemoreillon.com/whoami',
    }
  }),
};
</script>
