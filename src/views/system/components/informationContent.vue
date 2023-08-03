<template>
  <div id="informationContent" v-loading="loading">
    <div v-if="!fileViewer">
      <div class="dataSearch">
        <el-input
          class="dataInput"
          v-model="qureyParame.informationName"
          @keyup.enter.native="handleSearch"
          clearable
          placeholder="请输入关键字"
        >
          <i slot="suffix" class="el-icon-search" @click="handleSearch"></i>
        </el-input>
      </div>
      <div class="item" v-if="listData.length > 0">
        <div class="items-content" v-for="items in listData" :key="items.id">
          <div style="display: flex; flex: 1; cursor: pointer">
            <img src="../../../assets/fileIcon.png" style="margin: 0 20px" />
            <div style="color: #3685ea" @click="handleFileViewer(items)">{{ items.informationName }}</div>
          </div>
          <div style="font-size: 14px; color: #919aa8; padding: 0 20px">{{ items.releaseTime }}</div>
        </div>
      </div>
      <div v-else class="no-data">暂无数据</div>
      <div class="pagination-box">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="qureyParame.pageNum"
          :page-sizes="pageSizes"
          :page-size="qureyParame.pageSize"
          layout="total, sizes, prev, pager, next, jumper"
          background
          :total="total"
        >
        </el-pagination>
      </div>
    </div>
    <fileViewer :fileViewerData="fileViewerData" v-if="fileViewer" @handleFileViewerClone="handleFileViewerClone" />
  </div>
</template>

<script>
import fileViewer from './fileViewer.vue'
export default {
  name: 'informationContent',
  components: {
    fileViewer,
  },
  props: {
    listData: {
      Array,
      require: true,
    },
    qureyParame: {
      Object,
      require: true,
    },
    total: {
      Number,
      require: true,
    },
  },
  data() {
    return {
      fileViewer: false,
      fileViewerData: {},
      loading: false,
      pageSizes: [10, 20, 30, 50],
    }
  },
  methods: {
    handleFileViewerClone(flag) {
      this.fileViewer = flag
    },
    handleFileViewer(items) {
      this.fileViewer = true
      items.createdTime = this.formatTime(items.createdTime)
      this.fileViewerData = items
    },
    handleSearch() {
      this.$emit('handleSearch', this.qureyParame)
    },
    handleSizeChange(pageSize) {
      this.qureyParame.pageSize = pageSize
      this.$emit('handleSearch', this.qureyParame)
    },
    handleCurrentChange(currentPage) {
      this.qureyParame.pageNum = currentPage
      this.$emit('handleSearch', this.qureyParame)
    },
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
.no-data {
  text-align: center;
  padding: 10px 0;
  background-color: #edf1f6;
}
</style>