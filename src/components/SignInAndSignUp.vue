<template>
  <div id="signInAndSignUp">
    <h1>TodoList</h1>
    <div class="loginMethods">
      <div class="btn" @click="actionType='signIn'" :class="{active: actionType==='signIn'}">
        登录
      </div>
      <div class="btn" @click="actionType='signUp'" :class="{active: actionType==='signUp'}">
        注册
      </div>
    </div>
    <div :class="actionType" :key="actionType">
      <form @submit.prevent="actionType==='signIn'?login():signUp()">
        <div class="formRow">
          <span>用户名：</span>
          <input class="t-input" type="text" v-model="formData.username">
        </div>
        <div class="formRow">
          <span>密码：</span>
          <input class="t-input" type="password" v-model="formData.password">
        </div>
        <div class="formAction">
          <button type="submit" class="btn">{{ actionType === 'signIn' ? '登录' : '注册' }}</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
  import AV from '../assets/AV'

  export default {
    data: function () {
      return {
        actionType: 'signIn',
        formData: {
          username: '',
          password: ''
        }
      }
    },
    methods: {
      login() {
        AV.User.logIn(this.formData.username, this.formData.password).then(loginUser => {
          this.$emit('updateUser')
          console.log('登录成功', loginUser)
        }).catch(error => {
          console.log('登录失败', error)
          alert('请输入正确的用户名和密码！')
        })
      },
      signUp() {
        let user = new AV.User()
        user.setUsername(this.formData.username)
        user.setPassword(this.formData.password)
        user.signUp().then(loginUser => {
          this.$emit('updateUser')
          console.log('注册成功', loginUser)
        }).catch(error => {
          console.log('注册失败', error)
          alert('注册失败，用户名可能被占用！')
        })
      }
    }
  }
</script>

<style lang="scss">
  #signInAndSignUp {
    h1 {
      margin-bottom: 80px;
      font-size: 48px;
      text-align: center;
      color: #12b7f5;
    }
    .loginMethods {
      margin-bottom: 36px;
      display: flex;
      .btn {
        flex: 1;
        border-radius: 0;
        background-color: #353640;
        color: #fff;
        &:hover {
          background-color: #12b7f5;
        }
        &.active {
          border-bottom: 2px solid #12b7f5;
        }
      }
    }
    .formRow {
      margin-top: 24px;
      span {
        color: #fff;
      }
    }
    .formAction{
      margin-top: 16px;
      text-align: center;
    }
  }
</style>
