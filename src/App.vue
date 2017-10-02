<template>
  <div id="app">
    <aside id="aside">
      <h1>TodoList</h1>
      <SignInAndSignUp v-if="!currentUser" @updateUser="updateUser"/>
      <button @click="logout">tuichu</button>
    </aside>

  </div>
</template>

<script>
  import TodoView from './components/TodoView.vue'
  import TodoInput from './components/TodoInput.vue'
  import SignInAndSignUp from './components/SignInAndSignUp.vue'
  import AV from './assets/AV'

  export default {
    name: 'app',
    components: {TodoView, TodoInput, SignInAndSignUp},
    data: function () {
      return {
        currentUser: null,
        todoList: []
      }
    },
    methods: {
      updateUser() {
        this.currentUser = this.getCurrentUser()
      },
      getCurrentUser() {
        let user = AV.User.current()
        if(user) {
          let {id, createdAt, attributes: {username}} = user
          return {id, createdAt, username}
        }
        return null
      },
      logout() {
        AV.User.logOut()
        this.currentUser = this.getCurrentUser()
      }
    },
    created() {
      this.currentUser = this.getCurrentUser()
    }
  }
</script>

<style lang="scss">
  #app {
    min-width: 800px;
    min-height: 500px;
    height: 100%;
    display: flex;
  }

  #aside {
    width: 300px;
    padding: 32px 48px;
    background-color: #292a33;
    h1 {
      font-size: 48px;
      text-align: center;
      color: #12b7f5;
    }
  }
</style>
