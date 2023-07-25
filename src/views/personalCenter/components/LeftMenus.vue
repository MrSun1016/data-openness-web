<template>
  <div id="menu-box">
    <div class="menus">
      <div
        v-for="(item, index) in menusList"
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
</template>
  
  <script>
export default {
  name: 'informationMenu',
  data() {
    return {
      menusList: [
        { id: '1', name: '基本信息' },
        { id: '2', name: '我的服务' },
        { id: '3', name: '我的应用' },
        { id: '4', name: '我的收藏' },
        { id: '5', name: '我的评价' },
      ],
      selectIndex: 0,
    }
  },
  methods: {
    handleSelect(i) {
      this.selectIndex = i
      this.$emit("handleSelect", i);
    },
  },
}
</script>
  
  <style lang="less" scoped>
#menu-box {
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