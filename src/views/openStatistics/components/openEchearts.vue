<template>
  <div id="openEchearts">
    <!-- 头部 -->
    <div class="header"></div>
    <div class="max"></div>
  </div>
</template>
<script>
export default {
  name: 'openEchearts',
  data() {
    return {}
  }
}
</script>

<style lang="less" scoped>
#openEchearts {
  width: 100%;
  height: 100%;
  background-image: url(~@/assets/images/背景.jpg);
  background-size: 100% 100%;
  .header {
    width: 100%;
    height: 10%;
    background-color: rgba(226, 160, 160, 0.5);
  }
}
</style>
