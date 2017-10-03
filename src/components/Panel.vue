<template>
  <div class="panel">
    <div class="title">
      {{ todo.title }}
    </div>
    <div class="detail">
      {{ todo.detail }}
    </div>
    <div class="info">
      <span>{{ todo.done? '已完成' : '未完成' }}</span>
      <span>{{ todo.createdAt }}</span>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['activeTodo', 'todoList'],
    computed: {
      todo: function () {
        if(this.activeTodo) {
          return this.activeTodo
        }
        if(this.todoList.doing[0]) {
          return this.todoList.doing[0]
        } else {
          return {
            title: '详情面板',
            title: '详情面板',
            detail: '内容描述',
            createdAt: new Date().toLocaleString(),
            done: false,
          }
        }
      }
    }
  }
</script>

<style lang="scss">
  .panel {
    border-radius: 8px;
    overflow: hidden;
    .title {
      text-align: center;
      background-color: #12b7f5;
      min-height: 32px;
      max-height: 96px;
      overflow: auto;
      line-height: 32px;
      font-size: 16px;
      color: #fff;
    }
    .detail {
      background-color: #fff;
      height: 100px;
      overflow: auto;
      color: #333;
      padding: 8px;
    }
    .info {
      display: flex;
      justify-content: space-between;
      padding: 8px 16px;
      background-color: #12b7f5;
    }
  }
</style>
