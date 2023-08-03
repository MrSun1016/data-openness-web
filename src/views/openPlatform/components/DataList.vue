<template>
  <div id="DataList">
    <!-- 导航 -->
    <div class="nav">
      <div class="nav-title-content">
        <div
          class="title"
          :class="{ active: currentSelect === index }"
          v-for="(navs, index) in navList"
          :key="navs.id"
          @click="handleSelect(index)"
          v-show="currtSelect === 0"
        >
          {{ navs.title }}
        </div>
        <div style="background: #f5f5f9; line-height: 35px; padding: 0 8px" v-show="currtSelect === 1">
          最新一个月接口调用TOP
        </div>
        <div style="background: #f5f5f9; line-height: 35px; padding: 0 8px" v-show="currtSelect === 2">
          最新一个月数据下载TOP
        </div>
      </div>
      <div class="move">更多<i class="el-icon-d-arrow-right"></i></div>
    </div>
    <!-- 内容 -->
    <div class="content">
      <div class="info" v-for="item in listData" :key="item.id">
        <div class="info-title">
          <img style="margin: 0 8px" src="../../../assets/images/newDataImg.png" alt="" />
          <div style="color: #006fff">{{ item.catalogName || '-' }}}</div>
        </div>
        <div class="info-content">
          <div style="flex: 1">数源部门：{{ item.applicationDepartment || '-' }}</div>
          <div style="flex: 1">数源领域：{{ item.ly || '-' }}</div>
          <div style="flex: 1">开放时间：{{ item.openTime || '-' }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DataList',
  props: {
    listData: {
      Array,
      require: true,
    },
    currtSelect: {
      Number,
      require: true,
    },
  },
  data() {
    return {
      currentSelect: 0,
      navList: [
        { id: '1', title: '最新开放' },
        { id: '2', title: '最近更新' },
      ],
      newDataList: [
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
#DataList {
  width: 100%;
  padding: 0 30px;
  .nav {
    height: 37px;
    display: flex;
    justify-content: space-between;
    border-bottom: 1px solid #2c55b4;
    .nav-title-content {
      cursor: pointer;
      display: flex;
      height: 37px;
      align-items: center;
      .title {
        text-align: center;
        width: 125px;
        line-height: 35px;
        align-items: center;
      }
    }
    .move {
      cursor: pointer;
      font-size: 12px;
      color: #006fff;
    }
  }
  .content {
    .info {
      height: 82px;
      display: flex;
      flex-direction: column;
      justify-content: space-around;
      border-bottom: 1px solid #d8d8d8;
      .info-title {
        display: flex;
      }
      .info-content {
        display: flex;
        padding: 0 8px;
        justify-content: space-around;
      }
    }
  }
  .active {
    background: #f5f5f9;
    border-bottom: 2px solid #6190f3;
  }
}
</style>