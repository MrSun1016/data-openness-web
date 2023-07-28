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
      <div class="items-content" v-for="items in listData" :key="items.id">
        <div style="display: flex; flex: 1; cursor: pointer">
          <img src="../../../assets/fileIcon.png" style="margin: 0 20px" />
          <div style="color: #3685ea">{{ items.title }}</div>
        </div>
        <div style="font-size: 14px; color: #919aa8; padding: 0 20px">{{ items.createTime }}</div>
      </div>
    </div>
    <div class="pagination-box">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="learning.pageNum"
        :page-sizes="[10, 20, 30, 50]"
        :page-size="learning.pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        background
        :total="total"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
export default {
  name: 'informationContent',
  props:{
    listData:Array,
    require:true
  },
  data() {
    return {
      learning: {
        uploadFileName: '',
        pageNum: 1,
        pageSize: 10,
      },
      total: 0,
    }
  },
  methods: {
    handleSearch() {},
    handleSizeChange() {},
    handleCurrentChange() {},
  },
}
</script>
<style>
</style>
<style lang="less" scoped>
#informationContent {
  width: 100%;
  height: calc(100vh - 105px);
  display: flex;
  flex-direction: column;
  padding: 0 24px;
  .item {
    flex: 1;
    overflow: auto;
    background-color: #edf1f6;
    .items-content {
      height: 80px;
      display: flex;
      align-items: center;
      border-bottom: 1px solid #e8e8e8;
    }
  }
  .pagination-box {
    margin: 10px 0;
    text-align: center;
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