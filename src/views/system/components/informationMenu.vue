<template>
  <div class="information-menu">
    <div id="informationMenu">
      <div class="menus">
        <div
          v-for="(item, index) in menus"
          :key="item.id"
          class="items"
          :class="{ active: index === selectIndex }"
          @click="handleSelect(index)"
        >
          <div class="rectangle" v-if="index === selectIndex"></div>
          <div style="margin: 0 5px">{{ item.name }}</div>
        </div>
      </div>
    </div>
    <informationContent  :listData="listData"/>
  </div>
</template>

<script>
import informationContent from '@/views/system/components/informationContent'
export default {
  name: 'informationMenu',
  components: {
    informationContent,
  },
  props: {
    menus: Array,
    // default: ['', ''],
    require: true,
  },
  data() {
    return {
      selectIndex: 0,
      listData: [
        {
          id: '1',
          title: '关于印发《孝感市公告数据开放管理暂行办法》的通知',
          createTime: '2023-07-25 11:01:20',
        },
        {
          id: '2',
          title: '关于印发《孝感市公告数据开放管理暂行办法》的通知',
          createTime: '2023-07-25 11:01:20',
        },
        {
          id: '3',
          title: '关于印发《孝感市公告数据开放管理暂行办法》的通知',
          createTime: '2023-07-25 11:01:20',
        },
        {
          id: '4',
          title: '关于印发《孝感市公告数据开放管理暂行办法》的通知',
          createTime: '2023-07-25 11:01:20',
        },
        {
          id: '5',
          title: '关于印发《孝感市公告数据开放管理暂行办法》的通知',
          createTime: '2023-07-25 11:01:20',
        },
        {
          id: '6',
          title: '关于印发《孝感市公告数据开放管理暂行办法》的通知',
          createTime: '2023-07-25 11:01:20',
        },
        {
          id: '7',
          title: '关于印发《孝感市公告数据开放管理暂行办法》的通知',
          createTime: '2023-07-25 11:01:20',
        },
        {
          id: '8',
          title: '关于印发《孝感市公告数据开放管理暂行办法》的通知',
          createTime: '2023-07-25 11:01:20',
        },
        {
          id: '9',
          title: '关于印发《孝感市公告数据开放管理暂行办法》的通知',
          createTime: '2023-07-25 11:01:20',
        },
        {
          id: '10',
          title: '关于印发《孝感市公告数据开放管理暂行办法》的通知',
          createTime: '2023-07-25 11:01:20',
        },
      ],
    }
  },
  methods: {
    handleSelect(i) {
      this.selectIndex = i
    },
  },
}
</script>

<style lang="less" scoped>
.information-menu {
  width: 100%;
  display: flex;
}
#informationMenu {
  width: 30%;
  .menus {
    // width: 280px;
    // margin: 0 auto;
    cursor: pointer;
    border-radius: 5px;
    background: rgba(246, 246, 249);
    // background: #919aa8;
    .items {
      padding-left: 24px;
      display: flex;
      align-items: center;
      height: 52px;
      line-height: 52px;
    }
    .rectangle {
      width: 3px;
      height: 20px;
      display: inline-block;
      background: #fff;
    }
  }
  .active {
    color: #fff;
    background: #4888ff;
    border-radius: 5px 5px 0 0;
  }
}
</style>