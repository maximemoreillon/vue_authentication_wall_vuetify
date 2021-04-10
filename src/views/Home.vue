<template>
  <div class="home">
    <h1>Home</h1>
    <p>{{user}}</p>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'Home',
  data(){
    return {
      user: null,
    }
  },
  mounted(){
    //console.log(this.axios.defaults.headers.common)
    this.axios.get(`https://api.authentication.maximemoreillon.com/whoami`)
    .then( ({data}) => {
      this.user = data
    })
    .catch(error => {
      console.error(error)
    })
  }
}
</script>
