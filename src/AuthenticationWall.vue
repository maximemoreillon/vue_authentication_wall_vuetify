<template>
  <div class="wall_wrapper">

    <transition name="fade">
      <div class="wall" v-if="!logged_in">
        <LoginForm :options="options"/>
      </div>
    </transition>


    <slot v-if="logged_in"/>


  </div>
</template>

<script>
// @ is an alias to /src
import LoginForm from '@/components/LoginForm.vue'

export default {
  name: 'Home',
  components: {
    LoginForm,
  },
  props: {
    options: String,
  },
  data(){
    return {
      logged_in: false,
    }
  },
  methods:{
    login(){
      this.logged_in = true
    }
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
