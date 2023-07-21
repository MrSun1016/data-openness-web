<template>
  <div id="CatalogueTable">
    <!-- 头部 -->
    <div class="table-header">
      <div :class="{ active: headercolo === '1' }" @click="handleCataloguesSort('updateTime')" style="fontweight: 500">
        发布时间
      </div>
      <div
        :class="{ active: headercolo === '2' }"
        @click="handleCataloguesSort('notionalPoolingNum')"
        style="fontweight: 500"
      >
        更新时间
      </div>
      <div :class="{ active: headercolo === '3' }" @click="handleCataloguesSort('viewNum')" style="fontweight: 500">
        热门推荐
      </div>
      <div :class="{ active: headercolo === '4' }" @click="handleCataloguesSort('viewNum')" style="fontweight: 500">
        访问量
      </div>
      <div :class="{ active: headercolo === '5' }" @click="handleCataloguesSort('viewNum')" style="fontweight: 500">
        下载量/申请量
      </div>
      <div :class="{ active: headercolo === '6' }" @click="handleCataloguesSort('viewNum')" style="fontweight: 500">
        评分
      </div>
      <!-- <el-button type="text" class="items" style="fontWeight: 500">数据目录{{ total }}个</el-button> -->
    </div>
    <!-- 头部End -->
    <!-- 内容区 -->
    <div class="table-content" v-if="catalogueTableData.length" v-loading="tableLoading">
      <div v-for="item in catalogueTableData" :key="item.id">
        <div class="content-left">
          <div class="info">
            <div class="info-items">
              <div class="info-tltile">高中学校信息</div>
              <div>评分：7.8分</div>
            </div>
            <div class="info-center">
              <div>
                <img src="" alt="">
              </div>
              <div class="info-information">
                <div class="divinformation">来源部门：<span>教育局</span></div>
                <div class="divinformation">目录发布时间：<span>教育局</span></div>
                <div class="divinformation">申请量：<span>教育局</span></div>
                <div class="divinformation">开放等级：<span>教育局</span></div>
                <div class="divinformation">数据领域：<span>教育局</span></div>
                <div class="divinformation">数据更新时间：<span>教育局</span></div>
                <div class="divinformation">访问量：<span>教育局</span></div>
              </div>
              <div class="iconPointedStar">
                <img src="~@assets/images/收藏.png" alt="" />
                <!-- <img src="~@assets/images/五角星空.png" alt="" /> -->
              </div>
            </div>
          </div>
        </div>
        <!-- <el-divider></el-divider> -->
      </div>
    </div>
    <div class="divdata" v-else v-loading="tableLoading"><p>暂无数据</p></div>
    <!-- <div v-show="loading">数据正在加载中...</div> -->
    <!-- 申请接口-库表 -->
    <CatalogueDrawer ref="drawer" />
    <!-- 申请归集弹窗 -->
    <ApplyForCollectionDialog ref="dialog" />
  </div>
</template>

<script>
import { getDataUseInfo } from '@/api/api'
import CatalogueDrawer from '@views/catalogue/components/CatalogueDrawer'
import ApplyForCollectionDialog from '@views/catalogue/components/ApplyForCollectionDialog'
export default {
  name: 'CatalogueTable',
  components: {
    CatalogueDrawer,
    ApplyForCollectionDialog
  },
  props: ['catalogueTableData', 'serviceName'],
  data() {
    return {
      tableLoading: false,
      timeSortAct: 1,
      catalogSort: 0,
      browseSort: 0,
      dataRanges: [],
      dataRangeList: [],
      dataRangeItem: '',
      sortType: '',
      num: '',
      headercolo: '1'
    }
  },
  activated() {
    // 初始化排序
    // this.initSort()
    let querySortType = this.$route.query.type
    this.num = this.$route.query.num
    if (this.num) this.timeSortAct = 0
    else this.timeSortAct = 1
    this.handleCataloguesSort(querySortType)
  },
  created() {
    // this.handleCataloguesSort(querySortType)
  },
  mounted() {},
  computed: {},
  methods: {
    // 跳详情
    toDetails() {
      this.$router.push({
        path: '/catalogue/datacatalogue/details',
        query: {}
      })
    },

    onApplyForCollection(id) {
      this.$refs.dialog.collectionDialog = true
      this.$refs.dialog.catalogId = id
    },
    handleCataloguesSort(sortType) {
      this.sortType = sortType
      switch (this.sortType) {
        case 'updateTime':
          this.catalogSort = 0
          this.browseSort = 0
          this.timeSortAct++
          if (this.timeSortAct === 1) {
            if (this.serviceName == '事项') {
              this.$emit('handleMatterSort', sortType, 'desc')
            } else {
              // 升序
              this.$emit('handleSort', sortType, 'desc')
            }
          } else if (this.timeSortAct === 2) {
            if (this.serviceName == '事项') {
              this.$emit('handleMatterSort', sortType, 'asc')
            } else {
              // 倒序
              this.$emit('handleSort', sortType, 'asc')
            }
          } else {
            this.timeSortAct = 1
            if (this.serviceName == '事项') {
              this.$emit('handleMatterSort', sortType, 'desc')
            } else {
              this.$emit('handleSort', sortType, 'desc')
            }
          }
          break
        case 'notionalPoolingNum':
          this.timeSortAct = 0
          this.browseSort = 0
          this.catalogSort++
          if (this.timeSortAct === 1) {
            if (this.serviceName == '事项') {
              this.$emit('handleMatterSort', sortType, 'desc')
            } else {
              // 升序
              this.$emit('handleSort', sortType, 'desc')
            }
          } else if (this.catalogSort === 2) {
            if (this.serviceName == '事项') {
              this.$emit('handleMatterSort', sortType, 'asc')
            } else {
              // 倒序
              this.$emit('handleSort', sortType, 'asc')
            }
          } else {
            this.catalogSort = 1
            if (this.serviceName == '事项') {
              this.$emit('handleMatterSort', sortType, 'desc')
            } else {
              this.$emit('handleSort', sortType, 'desc')
            }
          }
          break
        case 'viewNum':
          this.timeSortAct = this.catalogSort = 0
          this.browseSort++
          if (this.browseSort === 1) {
            if (this.serviceName == '事项') {
              this.$emit('handleMatterSort', sortType, 'desc')
            } else {
              // 升序
              this.$emit('handleSort', sortType, 'desc')
            }
          } else if (this.browseSort === 2) {
            if (this.serviceName == '事项') {
              this.$emit('handleMatterSort', sortType, 'asc')
            } else {
              // 倒序
              this.$emit('handleSort', sortType, 'asc')
            }
          } else {
            this.browseSort = 1
            if (this.serviceName == '事项') {
              this.$emit('handleMatterSort', sortType, 'desc')
            } else {
              this.$emit('handleSort', sortType, 'desc')
            }
          }
          break
      }
    },
    goCatalogueDetail(itemData) {
      this.$router.push({
        path: '/catalogue/datacatalogue/details',
        query: {
          id: itemData.id,
          interfaceIsMount: itemData.interfaceIsMount,
          tableIsMount: itemData.tableIsMount,
          serviceName: this.serviceName,
          isHide: itemData.isHide,
          dataFormat: itemData.dataFormat,
          isHistory: itemData.isHistory,
          relatedGovernment: itemData.relatedGovernment
        }
      })
      localStorage.setItem('catalogName', itemData.catalogName)
      let catalogId = itemData.id
      // let catalogId = "307020400930030000/000085"
      getDataUseInfo(catalogId).then(res => {
        if (res.success) {
          console.log(res, '1111111')
          this.$bus.$emit('onExatableData', res.body)
        }
      })
    },
    handeleDrawer(title, item) {
      this.$refs.drawer.catalogId = item.id
      this.$refs.drawer.catalogName = item.catalogName
      this.$refs.drawer.title = title
      this.$refs.drawer.drawer = true
      this.$refs.drawer.interfaceInit()
      if (title == '库表') this.$refs.drawer.fetchDataItemInfo()
      else return
    }
  }
}
</script>

<style lang="less" scoped>
#CatalogueTable {
  margin-top: 10px;
  .table-header {
    height: 45px;
    color: #333333;
    display: flex;
    align-items: center;
    background: #f6f6f9;
    div {
      margin: 0 35px;
      display: flex;
      align-items: center;
      cursor: pointer;
    }
  }
  .table-content {
    .content-left {
      // display: flex;
      // justify-content: space-between;
      // align-items: center;
      padding: 18px 33px 0 32px;
      background-color: #fafafb;
      border-bottom: 1px solid #f3f5f8;
      .info-items {
        margin-bottom: 15px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        .info-tltile {
          font-size: 20px;
          color: #191c20;
        }
      }
      .info-center {
        display: flex;
        align-items: center;
        justify-content: space-between;
        .info-information {
          width: 90%;
          display: flex;
          flex-wrap: wrap;
          .divinformation {
            width: 200px;
            color: #919aa8;
            margin: 0 0 15px 0;
            span {
              color: #4f5967;
            }
          }
        }
        .iconPointedStar {
          display: flex;
          flex-direction: row-reverse;
          width: 10%;
          img {
            width: 32px;
          }
        }
      }
      .dataPreview {
        margin-bottom: 10px;
      }
    }
  }
  .divdata {
    width: 100%;
    margin-top: 80px;
    p {
      font-size: 16px;
      color: darkgrey;
      text-align: center;
    }
  }
  .small-mask-info {
    height: 150px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .title {
    height: 60px;
    color: #1890ff;
    line-height: 60px;
    padding-left: 32px;
    font-weight: 500;
    border-bottom: 1px solid #1890ff;
  }

  /deep/ .el-dialog__body {
    padding: 0 0;
  }
  /deep/ .el-dialog__header {
    padding: 0 0;
  }
  .deletmarg {
    margin-right: 20px;
  }
}
/deep/.el-divider--horizontal {
  margin: 0;
}
.active {
  color: #1890ff;
}
</style>
