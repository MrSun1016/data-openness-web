<template>
  <div id="homehotlist">
    <div class="left-nav">
      <div
        class="item-info"
        v-for="(navs, index) in navList"
        :key="navs.id"
        @click="handleSelect(index)"
        :class="{ active: currentSelect === index }"
      >
        <img class="img" :src="currentSelect === index ? navs.actImgUrl : navs.imgUrl" />
        <div>
          <div class="nav-title">{{ navs.navTitle }}</div>
          <div class="eng-title">{{ navs.engTitle }}</div>
        </div>
      </div>
    </div>
    <!-- 右侧内容 -->
    <DataList :listData="listData" />
  </div>
</template>

<script>
import DataList from '@/views/openPlatform/components/DataList'
import { getHomeHotBrowse, getHomeLatestData, getHomeHotApply, getDataUseInfo } from '@/api/api'
export default {
  name: 'homehotlist',
  components: {
    DataList,
  },
  data() {
    return {
      listData: [
        {
          id: '1',
          title: '孝感市好差评数据',
          dep: '部门',
          ly: '领域',
          sql: '12313',
          fw: '123123',
          time: '2023-07-28',
        },
        {
          id: '2',
          title: '孝感市好差评数据',
          dep: '部门',
          ly: '领域',
          sql: '12313',
          fw: '123123',
          time: '2023-07-28',
        },
        {
          id: '3',
          title: '孝感市好差评数据',
          dep: '部门',
          ly: '领域',
          sql: '12313',
          fw: '123123',
          time: '2023-07-28',
        },
        {
          id: '4',
          title: '孝感市好差评数据',
          dep: '部门',
          ly: '领域',
          sql: '12313',
          fw: '123123',
          time: '2023-07-28',
        },
        {
          id: '5',
          title: '孝感市好差评数据',
          dep: '部门',
          ly: '领域',
          sql: '12313',
          fw: '123123',
          time: '2023-07-28',
        },
      ],
      currentSelect: 0,
      showTooltip: false,
      navList: [
        {
          id: '1',
          navTitle: '最新数据',
          engTitle: 'NEW DATA',
          imgUrl: require('../../../assets/images/new.png'),
          actImgUrl: require('../../../assets/images/actNew.png'),
        },
        {
          id: '2',
          navTitle: '接口调用',
          engTitle: 'INTERFACE CALL',
          imgUrl: require('../../../assets/images/exp.png'),
          actImgUrl: require('../../../assets/images/actExp.png'),
        },
        {
          id: '3',
          navTitle: '数据下载',
          engTitle: 'DATA DOWNLOAD',
          imgUrl: require('../../../assets/images/dow.png'),
          actImgUrl: require('../../../assets/images/actDow.png'),
        },
      ],
    }
  },
  created() {},
  watch: {
    // HomeHotBrowseList: {
    //   handler() {
    //     this.$nextTick(() => this.checkWidth())
    //   },
    //   immediate: true,
    // },
  },
  methods: {
    handleSelect(i) {
      this.currentSelect = i
    },
  },
}
</script>

<style lang="less" scoped>
#homehotlist {
  display: flex;
  width: 1200px;
  margin: 20px auto 20px auto;
  .left-nav {
    .item-info {
      cursor: pointer;
      display: flex;
      align-items: center;
      width: 211px;
      // margin: 1px 0;
      height: 100px;
      background: #ebf3ff;
      &:nth-child(1) {
        border-bottom: 2px solid #e5e6e8;
      }
      &:nth-child(2) {
        border-bottom: 2px solid #e5e6e8;
      }
      .img {
        height: 24px;
        margin: 0 8px 13px 24px;
      }
      .nav-title {
        font-size: 18px;
        // color: black;
      }
      .eng-title {
        // color: #919aa8;
        font-size: 10px;
      }
    }
  }
  .left-nav .active {
    color: #006fff;
    background: #409eff42;
    border-radius: 6px 6px 0px 0px;
    box-shadow: 0px 4px 10px 0px #a9c8f6;
  }
  .textActive {
    color: #006fff !important;
  }
}
</style>
