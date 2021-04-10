<template>
  <div class="wall_wrapper">

    <transition name="fade">
      <div class="wall" v-if="!user">
        <LoginForm
          :options="options"
          @loggedIn="get_current_user()"/>
      </div>
    </transition>


    <slot v-if="!!user"/>


  </div>
</template>

<script>
// @ is an alias to /src
import LoginForm from '@/components/LoginForm.vue'
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
      user: null,
    }
  },
  mounted(){
    this.get_current_user()
  },
  methods:{

    get_current_user(){
      const jwt = VueCookies.get("jwt")

      if(!jwt) return

      const url = this.options.identification_url

      const headers = { Authorization: `Bearer ${jwt}` }

      axios.get(url, {headers})
      .then( ({data}) => {
        this.user = data
      })
      .catch( (error) => {
        console.error(error)
        VueCookies.remove('jwt')
        if(this.axios) delete this.axios.defaults.headers.common['Authorization']
       })

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
