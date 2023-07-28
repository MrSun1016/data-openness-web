<template>
  <div id="informationCard">
    <!-- title -->
    <div class="title">
      <div class="info">资讯</div>
      <div class="more">
        <i class="el-icon-view"></i>
        <div>查看更多</div>
      </div>
    </div>
    <div style="display: flex">
      <!-- nav -->
      <div class="nav">
        <div
          class="cards"
          :class="{ active: currentSelect === index }"
          v-for="(navs, index) in navsList"
          :key="navs.id"
          @click="handleSelect(index)"
        >
          {{ navs.title }}
        </div>
      </div>

      <!-- 右侧List -->
      <cardList :cardList="cardList" />
    </div>
  </div>
</template>

<script>
import cardList from '@/views/openPlatform/components/CardList'
export default {
  name: 'informationCard',
  components: {
    cardList,
  },
  data() {
    return {
      cardList: [
        { id: '1', titl: '我市首个24小时自助政务服务地方标准发布', time: '2023-07-28' },
        { id: '2', titl: '我市首个24小时自助政务服务地方标准发布', time: '2023-07-28' },
        { id: '3', titl: '我市首个24小时自助政务服务地方标准发布', time: '2023-07-28' },
        { id: '4', titl: '我市首个24小时自助政务服务地方标准发布', time: '2023-07-28' },
        { id: '5', titl: '我市首个24小时自助政务服务地方标准发布', time: '2023-07-28' },
        { id: '6', titl: '我市首个24小时自助政务服务地方标准发布', time: '2023-07-28' },
        { id: '7', titl: '我市首个24小时自助政务服务地方标准发布', time: '2023-07-28' },
        { id: '8', titl: '我市首个24小时自助政务服务地方标准发布', time: '2023-07-28' },
        { id: '9', titl: '我市首个24小时自助政务服务地方标准发布', time: '2023-07-28' },
      ],
      currentSelect: 0,
      navsList: [
        { id: '1', title: '新闻动态' },
        { id: '2', title: '政策法规' },
        { id: '3', title: '公告通知' },
      ],
    }
  },
  methods: {
    handleSelect(i) {
      this.currentSelect = i
    },
  },
}
</script>

<style lang="less" scoped>
#informationCard {
  width: 1200px;
  margin: 0 auto;
  margin-bottom: 15px;
  .title {
    margin-right: 30px;
    display: flex;
    justify-content: space-between;
    border-bottom: 2px solid #eef1f6;
    .info {
      width: 120px;
      font-size: 24px;
      color: #4888ff;
      border-bottom: 3px solid #2e68f6;
    }
    .more {
      cursor: pointer;
      display: flex;
      align-items: center;
      font-size: 14px;
      .el-icon-view {
        margin: 0 5px;
      }
    }
  }
  .nav {
    .cards {
      cursor: pointer;
      width: 211px;
      height: 56px;
      margin: 15px 0 25px 0;
      line-height: 56px;
      text-align: center;
      background: #79acff;
      color: #fff;
    }
  }

  .active {
    background: #2e6fea !important;
    box-shadow: 0px 4px 10px 0px #2e6fea;
  }
}
</style>