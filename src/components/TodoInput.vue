<template>
  <div id="todoInput">
    <div class="todoTitle">
      <label for="todoTitle">代办标题</label>
      <input class="t-input" id="todoTitle" type="text" placeholder="请添加任务" v-model="newTodo.title">
    </div>
    <div class="todoDetail">
      <label for="todoDetail">内容描述</label>
      <textarea class="t-input" id="todoDetail" rows="6" v-model="newTodo.detail" placeholder="详细内容，可省略。。。"></textarea>
    </div>
    <div class="addtodo">
      <button class="btn" @click="addTodo">添加</button>
    </div>
  </div>
</template>

<script>
  export default {
    data: function () {
      return {
        newTodo: {
          title: '',
          detail: ''
        },
      }
    },
    methods: {
      addTodo() {
        if(/\S/.test(this.newTodo.title)) {
          this.$emit('pushTodo', {
            title: this.newTodo.title,
            detail: this.newTodo.detail,
            createdAt: new Date().toLocaleString(),
            done: false,
          })
          this.newTodo.title = ''
          this.newTodo.detail = ''
        } else {
          alert('请添加标题')
        }
      }
    }
  }
</script>

<style lang="scss">
#todoInput {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  .todoDetail {
    margin-top: 16px;
  }
  label {
    color: #fff;
  }
  .addtodo {
    text-align: center;
  }
}
</style>
