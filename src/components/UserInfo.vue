<template>
  <div class="userInfo">
    <span>{{ currentUser.username }}</span>
    <button class="btn" @click="logout">退出</button>
  </div>

</template>

<script>
  import AV from '../assets/AV'

  export default {
    props: ['currentUser'],
    methods: {
      logout() {
        AV.User.logOut().then(() => {
          console.log('退出登录')
        })
        this.$emit('updateUser')
        window.location.reload()
      }
    }
  }
</script>

<style lang="scss">
 .userInfo {
   display: flex;
   justify-content: flex-end;
   align-items: center;
   color: #fff;
   span {
     font-size: 24px;
   }
   button {
     margin-left: 24px;
     color: #fff;
     background-color: #353640;
   }
 }
</style>
