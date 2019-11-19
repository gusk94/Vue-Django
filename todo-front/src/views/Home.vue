<template>
  <div class="home">

  </div>
</template>

<script>
import router from '@/router'

export default {
  name: 'Home',
  methods: {
    // 사용자 로그인 유무 -> 로그인 x -> 로그인 페이지로 보냄
    checkLogginIn() {
      // 1. 세션을 시작
      this.$session.start()

      // 2. 'jwt' 가 있는지 확인 -> boolean 반환
      if(!this.$session.has('jwt')){
        // Login Page
        router.push('/login')        
      }
    },
  },
  // Vue 가 화면에 그려지면 실행하는 함수
  mounted() {
    this.checkLogginIn()
  },
}
</script>

<style>

</style>