<template>
  <div class="wall_wrapper">

    <transition name="fade">

      <div class="wall" v-if="!loading && !user">
        <LoginForm
          :options="options"
          @loggedIn="get_current_user()"/>
      </div>

      <div class="wall" v-if="loading">
        <v-progress-circular
        indeterminate
        size="50"
        color="#444444" />
      </div>

    </transition>


    <slot v-if="!!user && !loading"/>


  </div>
</template>

<script>
// @ is an alias to /src
import LoginForm from './components/LoginForm.vue'
import axios from 'axios'
import VueCookies from 'vue-cookies'

export default {
  name: 'Home',
  components: {
    LoginForm,
  },
  props: {
    options: Object,
  },
  data(){
    return {
      loading: false,
      user: null,
    }
  },
  mounted(){
    this.get_current_user()
  },
  methods:{

    get_current_user(){

      // Does not feel like the right place to do this
      this.set_authorization_header()

      const jwt = VueCookies.get("jwt")

      if(!jwt) return

      this.loading = true

      const url = this.options.identification_url

      const headers = { Authorization: `Bearer ${jwt}` }

      axios.get(url, {headers})
      .then( ({data}) => {
        this.user = data
      })
      .catch( (error) => {
        console.error(error)
        VueCookies.remove('jwt')
        this.user = null
        if(this.axios) delete this.axios.defaults.headers.common['Authorization']
       })
       .finally( () => {
         this.loading = false
         this.$emit('user', this.user)
       })

    },
    set_authorization_header() {

      if(!this.axios) return

      const jwt = VueCookies.get("jwt")

      // either set or unset the header depending on of jwt being in cookies
      if(jwt) this.axios.defaults.headers.common['Authorization'] = `Bearer ${jwt}`
      else delete this.axios.defaults.headers.common['Authorization']

    },
  }
}
</script>

<style scoped>
.wall {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1000;
  background-color: white;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
