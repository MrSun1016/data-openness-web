<template>
  <!-- 内容区 -->
  <!-- 基本信息 -->
  <div class="content-box">
    <div class="flex-box">
      <div class="basic-title">数据收藏</div>
      <div class="conunt-box">
        <div class="count-items">数据量：{{ catalogDataDetails.notionalPoolingNum || '0' }}</div>
        <div class="count-items">浏览量：{{ catalogDataDetails.viewNum || '0' }}</div>
        <div class="count-items">申请量：{{ catalogDataDetails.applicationsNum || '0' }}</div>
        <div class="count-items">下载量：{{ catalogDataDetails.applicationsNum || '0' }}</div>
      </div>
    </div>
    <div class="basic-info-content">
      <div class="left-title" style="color: #333333">
        <p>
          <span class="itme-title">信息资源摘要：</span><span>{{ catalogDataDetails.keyAreasType || '-' }}</span>
        </p>

        <p>
          <span class="itme-title">领域分类：</span><span>{{ catalogDataDetails.sourceUnit || '-' }}</span>
        </p>

        <p>
          <span class="itme-title">数据范围：</span><span>{{ catalogDataDetails.shareType || '-' }}</span>
        </p>

        <p>
          <span class="itme-title">开放属性：</span><span>{{ catalogDataDetails.openType || '-' }}</span>
        </p>
      </div>

      <div class="left-title" style="color: #333333">
        <p>
          <span class="itme-title">发布时间：</span><span>{{ catalogDataDetails.keyAreasType || '-' }}</span>
        </p>

        <p>
          <span class="itme-title">应用系统：</span><span>{{ catalogDataDetails.sourceUnit || '-' }}</span>
        </p>

        <p>
          <span class="itme-title">关联类目：</span><span>{{ catalogDataDetails.shareType || '-' }}</span>
        </p>

        <p>
          <span class="itme-title">数源单位：</span><span>{{ catalogDataDetails.openType || '-' }}</span>
        </p>
      </div>

      <div class="left-title" style="flex-direction: column;">
        <p>
          <span class="itme-title">更新频率：</span><span>{{ catalogDataDetails.shareType || '-' }}</span>
        </p>

        <p>
          <span class="itme-title">更新时间：</span><span>{{ catalogDataDetails.openType || '-' }}</span>
        </p>
      </div>
    </div>
    <!-- 文件/接口tabs -->
    <CatalogueTabs :catalogDataDetails="catalogDataDetails" :shareApiInfo="shareApiInfo" :dataFormat="dataFormat" />
    <div class="divider"></div>
    <!-- 评论打分 -->
    <Commentscoring></Commentscoring>

    <!-- 申请归集弹窗 -->
    <ApplyForCollectionDialog ref="dialog" />
  </div>
</template>

<script>
import CatalogueTabs from '@views/catalogue/components/CatalogueTabs'
import Commentscoring from '@views/catalogue/components/Commentscoring'
import ApplyForCollectionDialog from '@views/catalogue/components/ApplyForCollectionDialog'
import EditCorrection from '@views/interaction/modules/EditCorrection'

import { getResourceMarket, getMatters, getDataUseApiInterfaceInfo, setDataErrorCorrection } from '@/api/api'
export default {
  components: {
    CatalogueTabs,
    Commentscoring,
    ApplyForCollectionDialog,
    EditCorrection
  },
  name: 'CatalogueDataDetails',
  data() {
    return {
      catalogDataDetails: {},
      dataFormat: '',
      isHide: false,
      interfaceIsMount: '',
      tableIsMount: '',
      id: '',
      shareApiInfo: {},
      dialogVisible: false,
      text: ''
    }
  },
  activated() {},
  mounted() {
    this.isHide = this.$route.query.isHide
    ;(this.interfaceIsMount = this.$route.query.interfaceIsMount),
      (this.tableIsMount = this.$route.query.tableIsMount),
      (this.isHistory = this.$route.query.isHistory),
      (this.relatedGovernment = this.$route.query.relatedGovernment)
    this.id = this.$route.query.id
    // this.isHide = this.$route.query.isHide
    this.dataFormat = this.$route.query.dataFormat
    this.fetchgetResourceMarket()
    this.fetchDataUseApiInterfaceInfo()
  },
  methods: {
    dataCorrection() {
      this.dialogVisible = true
      this.$refs.correction.getaddCorrection()
    },
    dataErrorCorrection() {
      let { text, id } = this
      if (text == '' || text.trim() == '') {
        this.dialogVisible = false
        this.$message.warning('纠错内容不能为空')
        return false
      }
      setDataErrorCorrection({ text, catalogId: id }).then(res => {
        if (res.success) {
          this.dialogVisible = false
          this.text = ''
          this.$message.success('提交成功!')
        } else {
          this.dialogVisible = false
          this.$message.error('提交失败!')
        }
      })
    },
    fetchDataUseApiInterfaceInfo() {
      getDataUseApiInterfaceInfo(this.id).then(res => {
        if (res.success) {
          this.shareApiInfo = res.body
        }
      })
    },
    // 申请归集
    handleCollection() {
      this.$refs.dialog.collectionDialog = true
      this.$refs.dialog.catalogId = this.$route.query.id
    },

    fetchgetResourceMarket() {
      getResourceMarket(this.id).then(async res => {
        if (res.success) {
          this.catalogDataDetails = await res.body
          this.catalogDataDetails.dataRange = res.body.dataRangeList
        }
      })
    },
    fetchMatters() {
      let id = this.$route.query.id
      getMatters(id).then(async res => {
        if (res.success) {
          this.catalogDataDetails = await res.body
          this.catalogDataDetails.dataRange = JSON.parse(this.catalogDataDetails.dataRange)
          this.catalogDataDetails.dataRange = this.catalogDataDetails.dataRange.flat(Infinity)
        }
      })
    },
    fetchResourceMarket() {
      let serviceName = this.$route.query.serviceName
      if (serviceName == '事项') {
        // 服务事项
        this.fetchMatters()
      } else {
        // 数据目录
        this.fetchgetResourceMarket()
      }
    }
  },
  activated() {
    this.fetchResourceMarket()
  }
  // created() {
  //   this.fetchResourceMarket()
  // },
}
</script>

<style lang="less" scoped>
@theme-color: #4888ff;
.content-box {
  width: 100%;
  background-color: #fff;
  .flex-box {
    display: flex;
    height: 40px;
    // padding: 0 32px;
    justify-content: space-between;
    align-items: center;
    border-bottom: 2px solid #f0f3f7;
    .basic-title {
      width: 90px;
      height: 40px;
      text-align: center;
      line-height: 40px;
      background-color: @theme-color;
      color: #fff;
      font-size: 15px;
    }
    .conunt-box {
      // width: 200px;
      display: flex;
      flex-direction: row;
      .count-items {
        width: 110px;
        height: 25px;
        color: #fff;
        font-size: 12px;
        text-align: center;
        line-height: 25px;
        margin-left: 15px;
        border-radius: 3px;
        background-color: #5f97ff;
      }
    }
  }
  .basic-info-content {
    display: flex;
    padding: 32px;
    justify-content: space-between;
    .left-title {
      width: 33%;
    }
  }
  .divider {
    width: 100%;
    margin: 40px 0;
    border: 2px dashed #f0f3f7;
  }
}

.server-info-content {
  display: flex;
  justify-content: space-between;
  padding: 32px;
  .lucency-box {
    width: 104px;
    background-color: #fff;
    color: #fff;
  }
  .server-itme-title {
    width: 160px;
    display: inline-block;
  }
}

/deep/ .cusa-btn {
  margin-left: 0px !important;
}
.itme-title {
  width: 160px;
  display: inline-block;
}
.item-df {
  display: flex;
}
</style>
