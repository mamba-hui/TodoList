<template>
  <div id="app">
    <aside id="aside">
      <h1>TodoList</h1>
      <SignInAndSignUp v-if="!currentUser" @updateUser="updateUser"/>
      <div id="inputAndUser" v-if="currentUser">
        <TodoInput @pushTodo="pushTodo"/>
        <UserInfo :currentUser="currentUser" @updateUser="updateUser"/>
      </div>
    </aside>
    <TodoView v-if="currentUser" :todoList="todoList" @removeTodo="removeTodo" @changeState="changeState" @toTop="toTop"/>
  </div>
</template>

<script>
  import TodoView from './components/TodoView.vue'
  import TodoInput from './components/TodoInput.vue'
  import SignInAndSignUp from './components/SignInAndSignUp.vue'
  import UserInfo from './components/UserInfo.vue'
  import AV from './assets/AV'

  export default {
    name: 'app',
    components: {TodoView, TodoInput, SignInAndSignUp, UserInfo},
    data: function () {
      return {
        currentUser: null,
        todoList: {
          id: undefined,
          doing: [],
          done: []
        }
      }
    },
    methods: {
      updateUser() {
        this.currentUser = this.getCurrentUser()
        this.fetchTodos()
      },
      getCurrentUser() {
        let user = AV.User.current()
        if(user) {
          let {id, createdAt, attributes: {username}} = user
          return {id, createdAt, username}
        }
        return null
      },
      pushTodo(newTodo) {
        this.todoList.doing.push(newTodo)
        this.saveOrUpdateTodos()
      },
      removeTodo(index) {
        this.todoList.done.splice(index, 1)
        this.saveOrUpdateTodos()
      },
      changeState(index, todo) {
        let state1 = todo.done === false ? 'doing' : 'done'
        let state2 = todo.done === true ? 'doing' : 'done'
        this.todoList[state1].push(todo)
        this.todoList[state2].splice(index, 1)
        this.saveOrUpdateTodos()
      },
      toTop(todo) {
        this.todoList.doing.sort((prev, cur) => {
          return cur === todo ? 1 : 0
        })
        this.saveOrUpdateTodos()
      },
      fetchTodos() {
        if(this.currentUser) {
          let query = new AV.Query('AllTodos')
          query.find().then(todos => {
            let avAllTodos = todos[0]
            this.todoList = JSON.parse(avAllTodos.attributes.content)
            console.log('fetch得到的数据', todos)
          }).catch(error => {
            console.log('fetch数据失败', error)
          })
        }
      },
      saveOrUpdateTodos() {
        if(this.todoList.id) {
          this.updateTodos()
        } else {
          this.saveTodos()
        }
      },
      saveTodos() {
        let dataString = JSON.stringify(this.todoList)
        let AVTodos = new AV.Object.extend('AllTodos')
        let avTodos = new AVTodos()
        avTodos.set('content', dataString)
        let acl = new AV.ACL()
        acl.setWriteAccess(AV.User.current(), true)
        acl.setReadAccess(AV.User.current(), true)
        avTodos.setACL(acl)
        avTodos.save().then(todos => {
          this.todoList.id = todos.id
          console.log('创建Todo并保存成功')
        }).catch(error => {
          console.log('创建Todo并保存失败', error)
          alert('创建时保存失败')
        })
      },
      updateTodos() {
        let dataString = JSON.stringify(this.todoList)
        let avTodos = AV.Object.createWithoutData('AllTodos', this.todoList.id)
        avTodos.set('content', dataString)
        avTodos.save().then(() => {
          console.log('数据更新成功')
        })
      }
    },
    created() {
      this.currentUser = this.getCurrentUser()
      this.fetchTodos()
    }
  }
</script>

<style lang="scss">
  #app {
    min-width: 1080px;
    min-height: 560px;
    height: 100%;
    display: flex;
  }

  #aside {
    display: flex;
    flex-direction: column;
    width: 300px;
    padding: 32px 48px;
    background-color: #292a33;
    h1 {
      font-size: 48px;
      text-align: center;
      color: #12b7f5;
    }
  }
  #inputAndUser {
    flex: 1;
    display: flex;
    flex-direction: column;
  }
</style>
