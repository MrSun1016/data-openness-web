<template>
  <div id="CatalogueHead" style="overflow-y: auto;">
    <!-- 头部 -->
    <div class="header">
      <div class="header-left">
        <i class="el-icon-arrow-left" @click="goBack"></i>
        <span style="vertical-align: super">高中学校信息</span>
      </div>
      <div class="header-right">
        <div class="collect-error">
          <i class="el-icon-star-off"></i>
          <!-- <img src="~@assets/images/五角星空.png" alt="" /> -->
          <span>数据收藏</span>
        </div>
        <div class="collect-error">
          <i class="el-icon-edit-outline"></i>
          <span>数据纠错</span>
        </div>
      </div>
    </div>
    <CatalogueDataDetails />
  </div>
</template>

<script>
import CatalogueDataDetails from '@views/catalogue/components/CatalogueDataDetails'
export default {
  name: 'CatalogueHead',
  components: {
    CatalogueDataDetails
  },
  data() {
    return {
      catalogName: ''
    }
  },
  methods: {
    goBack() {
      this.$router.back()
    }
  },
  activated() {
    // this.catalogName =  localStorage.getItem('catalogName')
  },
  mounted() {
    this.catalogName = localStorage.getItem('catalogName')
  }
}
</script>

<style lang="less" scoped>
@theme-color: #5F97FF;
#CatalogueHead {
  width: 90%;
  padding: 24px 24px;
  margin: 10px auto;
  // height: calc(100vh - 64px);
  .header {
    height: 62px;
    // line-height: 62px;
    background: #f6f6f9;
    // color: @theme-color;
    margin-bottom: 24px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .header-left {
      display: flex;
      align-items: center;
      margin-left: 10px;
      i {
        font-size: 24px;
        cursor: pointer;
        margin-right: 5px;
        color: black;
      }
      span {
        font-size: 20px;
        color: black;
      }
    }
    .header-right {
      height: 34px;
      display: flex;
      line-height: 34px;
      .collect-error {
        width: 110px;
        height: 34px;
        margin-right: 15px;
        background-color: @theme-color;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 3px;
        i {
          font-size: 22px;
          color: #fff;
        }
        span {
          font-size: 15px;
          color: #fff;
        }
        img {
          width: 20px;
        }
      }
    }
  }
}
</style>
