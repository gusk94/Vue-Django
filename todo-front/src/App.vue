<template>
  <div id="app">
    <div id="nav">
      <div v-if="isLoggedIn">
        <router-link to="/">Home</router-link> |
        <!-- <router-link to="/logout">Logout</router-link> -->
        <!-- prevent : href 로 redirect 방지 -->
        <a @click.prevent="logout" href="/logout">Logout</a>
      </div>
      <div v-else>
        <router-link to="/login">Login</router-link>
      </div>
    </div>
    <div class="container col-6">
      <router-view/>
    </div>
  </div>
</template>

<script>
import router from '@/router'

export default {
  name: 'App',
  data(){
    return {
      // jwt 있으면 true
      isLoggedIn: this.$session.has('jwt')
    }
  },
  methods: {
    logout(){
      this.$session.destroy()
      router.push('/login')
    },
  },
  // data 변화 -> 실행
  updated(){
    this.isLoggedIn = this.$session.has('jwt')
  }
}
</script>
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
