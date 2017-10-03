<template>
  <div id="banner" :class="{opacity: currentUser}">
    <h2>最简单的事件代办列表</h2>
    <div class="slogan">
      <b>高效率 慢生活</b>
      <br>
      <span>跟上时间的步伐 做时间的主人</span>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['currentUser']
  }
</script>

<style lang="scss">
  #banner {
    position: absolute;
    z-index: -1;
    left: 396px;
    right: 0;
    top: 0;
    bottom: 0;
    background: url("../assets/banner.jpg") no-repeat ;
    background-size: cover;
    h2 {
      color: #353d40;
      margin-left: 50px;
    }
    .slogan {
      position: absolute;
      top: 140px;
      left: 80px;
      b {
        font-weight:700;
        font-size: 50px;
        color: #1c1e1f;
        letter-spacing: 5px;
      }
      span {
        font-size: 18px;
        color: #353d40;
        line-height: 50px;
        padding-left: 5px;
      }
    }
    &.opacity {
      opacity: .2;
    }
  }
</style>
