<template>
  <div id="CatalogueTable">
    <el-tabs class="tabs" v-model="catalogueTableName">
      <el-tab-pane label="文件" name="cusa">
        <CatalogueCusa :catalogDataDetails="catalogDataDetails" />
      </el-tab-pane>
      <el-tab-pane label="接口" name="prot">
        <CataloguePort
          @handeleDrawer="handeleDrawer"
          :catalogDataDetails="catalogDataDetails"
          :shareApiInfo="shareApiInfo"
        />
      </el-tab-pane>
    </el-tabs>
    <CatalogueDrawer ref="drawer" />
  </div>
</template>

<script>
import CataloguePort from '@views/catalogue/components/CataloguePort'
import CatalogueCusa from '@views/catalogue/components/CatalogueCusa'
import CatalogueDrawer from '@views/catalogue/components/CatalogueDrawer'

export default {
  props: ['catalogDataDetails', 'dataFormat', 'shareApiInfo'],
  name: 'CatalogueTable',
  components: {
    CatalogueCusa,
    CataloguePort,
    CatalogueDrawer
  },
  data() {
    return {
      catalogueTableName: 'cusa'
    }
  },
  created() {},
  activated() {
    // this.initTabs()
  },
  mounted() {
    ;(this.interfaceIsMount = this.$route.query.interfaceIsMount), (this.tableIsMount = this.$route.query.tableIsMount)
    ;(this.isHistory = this.$route.query.isHistory), (this.relatedGovernment = this.$route.query.relatedGovernment)
  },
  methods: {
    handeleDrawer(row, title) {
      console.log('drawer')
      // this.id = this.$route.query.id
      // this.$refs.drawer.catalogName = row.catalogName
      // this.$refs.drawer.title = title
      this.$refs.drawer.drawer = true
      // this.$refs.drawer.cataId = this.id
      // this.$refs.drawer.catalogId = this.$route.query.id
      // this.$refs.drawer.fetchQueryAppName()
      // this.$refs.drawer.interfaceInit()
      // if (title == '库表') this.$refs.drawer.fetchDataItemInfo()
      // else return
    }
  }
}
</script>

<style lang="less" scoped>
#CatalogueTable {
  background: #fff;
  /deep/.el-tabs__item {
    background-color: #f6f6f9;
    color: #4888ff;
  }
  /deep/.el-tabs__item.is-active {
    background-color: #4888ff;
    color: #fff;
  }
  /deep/ .el-tabs__item {
    padding: 0px;
    width: 90px;
    text-align: center;
  }
  /deep/ .el-tabs__nav-wrap::after {
    background-color: #f6f6f9;
  }
  /deep/ .el-tabs__active-bar {
    width: 90px;
  }
}
</style>
