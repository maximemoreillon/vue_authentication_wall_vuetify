# Vue Authentication wall (Vuetify)

This components forces users to login before getting access to the main content of a VueJS app.

## Usage

```
<template>
  <v-app>
    <v-main>
      <AuthenticationWall :options="options">
        <router-view/>
      </AuthenticationWall>
    </v-main>
  </v-app>
</template>

<script>
import AuthenticationWall from '@moreillon/vue_authentication_wall_vuetify'
export default {
  name: 'App',
  components: {
    AuthenticationWall
  },
  data: () => ({
    options: {
      login_url: 'LOGIN URL',
      identification_url: 'IDENTIFICATION URL',
    }
  }),

}
</script>

```
