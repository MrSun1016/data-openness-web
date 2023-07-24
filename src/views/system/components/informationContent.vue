<template>
  <div id="informationContent">
    <div class="dataSearch">
      <el-input
        class="dataInput"
        v-model="learning.uploadFileName"
        @keyup.enter.native="handleSearch"
        clearable
        placeholder="请输入关键字"
      >
        <i slot="suffix" class="el-icon-search" @click="handleSearch"></i>
      </el-input>
    </div>
    <div class="item">
      <div class="items-content">
        <div style="display: flex; width: 70%; cursor: pointer">
          <img src="../../../assets/fileIcon.png" style="margin: 0 20px" />
          <div style="color: #3685ea">关于印发《孝感市公告数据开放管理暂行办法》的通知</div>
        </div>
        <div style="font-size: 14px; color: #919aa8">2023年07月24日14:46:17</div>
      </div>
      <div class="items-content">
        <div style="display: flex; width: 70%; cursor: pointer">
          <img src="../../../assets/fileIcon.png" style="margin: 0 20px" />
          <div style="color: #3685ea">关于印发《孝感市公告数据开放管理暂行办法》的通知</div>
        </div>
        <div style="font-size: 14px; color: #919aa8">2023年07月24日14:46:17</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'informationContent',
  data() {
    return {
      learning: {
        uploadFileName: '',
      },
    }
  },
  methods: {
    handleSearch() {},
  },
}
</script>
<style>
</style>
<style lang="less" scoped>
#informationContent {
  width: 100%;
  padding: 0 24px;
  .item {
    background-color: #edf1f6;
    .items-content {
      height: 80px;
      display: flex;
      align-items: center;
      border-bottom: 1px solid  #e8e8e8;
    }
  }
  .el-icon-search {
    line-height: 43px;
    width: 50px;
    cursor: pointer;
  }
  .dataSearch {
    width: 100%;
    height: 64px;
    display: flex;
    align-items: center;
    justify-content: space-around;
    background-color: #dde1e7;
    .dataInput {
      width: 95%;
    }
  }
}
</style>