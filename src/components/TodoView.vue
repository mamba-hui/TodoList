<template>
  <div id="todoView">
    <div class="layout">
      <div class="todos">
        <h2>正在进行</h2>
        <transition-group name="list" mode="out-in" tag="ol">
          <li v-for="(todo, index) in todoList.doing" :key="todo.createdAt">
            <input type="checkbox" v-model="todo.done" @click="changeState(index, todo)">
            <p>
              <span class="title" title="点击查看在左侧详情" @click="updateActiveTodo(todo)">{{ todo.title }}</span>
              <span class="time">{{ todo.createdAt }}</span>
              <button class="btn" @click="toTop(todo)">↑</button>
            </p>
          </li>
        </transition-group>
      </div>
      <div class="todosDone">
        <h2>已经完成</h2>
        <transition-group name="list" mode="out-in" tag="ol">
          <li v-for="(todo, index) in todoList.done" :key="todo.createdAt">
            <input type="checkbox" v-model="todo.done" @click="changeState(index, todo)">
            <p>
              <span class="title" title="点击查看在左侧详情" @click="updateActiveTodo(todo)">{{ todo.title }}</span>
              <span class="time">{{ todo.createdAt }}</span>
              <button class="btn" @click="removeTodo(index)">X</button>
            </p>
          </li>
        </transition-group>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['todoList'],
    methods: {
      removeTodo(index) {
        this.$emit('removeTodo', index)
      },
      changeState(index, todo) {
        this.$emit('changeState', index, todo)
      },
      toTop(todo) {
        this.$emit('toTop', todo)
      },
      updateActiveTodo(todo) {
        this.$emit('updateActiveTodo', todo)
      }
    }
  }
</script>

<style lang="scss">
  #todoView {
    flex: 1;
    background-color: rgba(0,0,0,.3);
    overflow-y: auto;
    .layout {
      width: 600px;
      margin: 0 auto;
      padding: 16px;
    }
    h2 {
      font-size: 24px;
    }
    .list-enter-active, .list-leave-active {
      transition: all .5s;
    }
    .list-enter, .list-leave-to{
      opacity: 0;
      transform: translateY(30px);
    }
    .list-move {
      transition: transform .5s;
    }
    li {
      height: 32px;
      line-height: 32px;
      background: #fff;
      position: relative;
      margin-bottom: 10px;
      padding-left: 40px;
      border-radius: 3px;
      border-left: 5px solid #629A9C;
      box-shadow: 0 1px 2px rgba(0, 0, 0, 0.07);
      overflow: hidden;
      input {
        position: absolute;
        top: 5px;
        left: 10px;
        width: 22px;
        height: 22px;
        cursor: pointer;
      }
      p {
        margin: 0;
        display: flex;
        .title {
          flex: 1;
          margin-right: 5px;
          border-radius: 3px;
          height: 32px;
          overflow: hidden;
          cursor: pointer;
          &:hover {
            background-color: #444;
            color: #fff;
          }
        }
        .btn {
          margin: 0;
          position: relative;
        }
      }
    }
  }
</style>
