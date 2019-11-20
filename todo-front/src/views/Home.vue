<template>
  <div class="home">
    <h1>Todo</h1>
    <TodoInput @createTodo="createTodo"/>
    <TodoList :todos="todos"/>
  </div>
</template>

<script>
import axios from 'axios'
// import jwtDecode from 'jwt-decode'
import { mapGetters } from 'vuex'
import TodoList from '../components/TodoList'
import TodoInput from '@/components/TodoInput'
import router from '@/router'

export default {
  name: 'Home',
  data() {
    return {
      todos: [],
    }
  },
  computed: {
    ...mapGetters([
      'isLoggedIn',
      'options',
      'userId'
    ])
  },
  components: {
    TodoList,
    TodoInput,
  },
  methods: {
    // 사용자 로그인 유무 -> 로그인 x -> 로그인 페이지로 보냄
    checkLogginIn() {
      // 1. 세션을 시작
      // this.$session.start()

      // // 2. 'jwt' 가 있는지 확인 -> boolean 반환
      // if(!this.$session.has('jwt')){
      //   // Login Page
      //   router.push('/login')        
      // }
      if (!this.isLoggedIn) {
        router.push('/login')
      }
    },
    // django API -> todos data
    getTodo() {
      // this.$session.start()
      // const token = this.$session.get('jwt')
      const SERVER_IP = process.env.VUE_APP_SERVER_IP
      // const userId = jwtDecode(token).user_id
      // const options = {
      //   headers: {
      //     Authorization: 'JWT ' + token
      //   }
      // }
      axios.get(`${SERVER_IP}/api/v1/users/${this.userId}/`, this.options)
        .then(response=>{
          this.todos = response.data.todo_set
        })
        .catch(error=>{
          console.log(error)
        })
    },
    createTodo(title) {
      // this.$session.start()
      // const token = this.$session.get('jwt')
      const SERVER_IP = process.env.VUE_APP_SERVER_IP
      // const options = {
      //   headers: {
      //     Authorization: 'JWT ' + token
      //   }
      // }
      // const userId = jwtDecode(token).user_id
      const data = {
        title,
        user: this.userId
      }
      axios.post(`${SERVER_IP}/api/v1/todos/`, data, this.options)
        .then(response=>{
          this.todos.push(response.data)
        })
        .catch(error=>{
          console.log(error)
        })
    }
  },
  // Vue 가 화면에 그려지면 실행하는 함수
  mounted() {
    if (this.isLoggedIn){
      this.checkLogginIn()
      this.getTodo()
    }
  },
  watch: {
    isLoggedIn() {
      this.getTodo()
    }
  }
}
</script>

<style>

</style>