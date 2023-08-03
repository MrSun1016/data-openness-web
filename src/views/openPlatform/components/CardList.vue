<template>
  <div id="cardList" class="right-content" v-loading="loading">
    <!-- 右侧内容 -->
    <div class="content-info" v-for="item in cardList" :key="item.id">
      <div>{{ item.informationName }}</div>
      <div>{{ item.releaseTime }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'cardList',
  props: {
    cardList: Array,
    require: true,
  },
  data(){
    return {
      loading:false
    }
  },
}
</script>

<style lang="less" scoped>
.right-content {
  width: 100%;
  margin-top: 15px;
  padding: 0 30px;
  .content-info {
    cursor: pointer;
    height: 40px;
    padding:0 5px;
    line-height: 40px;
    display: flex;
    justify-content: space-between;
    &:nth-child(2n-1) {
      background: #f0f2f4;
    }
  }
}
</style>