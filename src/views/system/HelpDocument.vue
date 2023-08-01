<template>
  <div id="helpdocument">
    <OpenPlatformHeader />
    <div class="content-box">
<<<<<<< HEAD
      <informationMenu class="informationMenu" :menus="menus" />
      <informationContent class="informationContent" />
=======
      <informationMenu :menus="menus" />
>>>>>>> da6a001f907c192aac3021cef9e100163c896829
    </div>
    <Openfooter />
  </div>
</template>
<<<<<<< HEAD
<script>
import informationContent from '@/views/system/components/informationContent'
=======
    <script>
import Openfooter from '@/components/page/Openfooter'
>>>>>>> da6a001f907c192aac3021cef9e100163c896829
import informationMenu from '@/views/system/components/informationMenu'
import OpenPlatformHeader from '@/components/page/OpenPlatformHeader'
import { studyWorldPage } from '@/api/api'
import { download } from '@/api/manage'
// import { Base64 } from 'js-base64'
import { Base64 } from '@/utils/base64.min.js'
export default {
  name: 'helpdocument',
  components: {
    OpenPlatformHeader,
    informationMenu,
<<<<<<< HEAD
    informationContent
=======
    Openfooter,
>>>>>>> da6a001f907c192aac3021cef9e100163c896829
  },
  data() {
    return {
      menus: [
        { id: '1', name: '新闻动态' },
        { id: '2', name: '政策法规' }
      ],
      loading: false,
      total: 0,
      todoList: [],
      learning: {
        dataName: '',
        id: 0,
        orderByField: '',
        pageNum: 1,
        pageSize: 10,
        rowStart: 0,
        releaseScope: [],
        startDate: '',
        endDate: '',
        uploadFileName: '',
        queryType: 2,
        dataManagementType: '1' //帮助文档
      },
      input: '',
      todoList: []
    }
  },
  created() {
    // this.fetchWorldPage()
  },
  methods: {
    // 文件下载
    downLoad(fileName) {
      // let fileName = this.datas.businessAeview.applyFlieName
      download({ fileName })
        .then(res => {
          const blob = new Blob([res], {
            type: 'application/json;charset=UTF-8'
          }) // res就是接口返回的文件流
          const link = document.createElement('a') // 创建a标签
          const objectUrl = window.URL.createObjectURL(blob)
          link.href = objectUrl
          link.download = fileName
          link.click()
          window.URL.revokeObjectURL(objectUrl) // 释放内存
        })
        .catch(error => {
          this.$message.warning('模板导出失败')
        })
    },
    preview(fileName) {
      var previewUrl = 'http://59.208.164.242:60034/demo/' + fileName
      window.open('http://59.208.164.242:60034/onlinePreview?url=' + encodeURIComponent(Base64.encode(previewUrl)))
    },
    handleSizeChange(pageSize) {
      this.learning.pageSize = pageSize
      this.fetchWorldPage()
    },
    handleCurrentChange(pageNum) {
      this.learning.pageNum = pageNum
      this.fetchWorldPage()
    },
    handleSearch() {
      this.fetchWorldPage()
    },
    fetchWorldPage() {
      this.loading = true
      studyWorldPage(this.learning)
        .then(res => {
          if (res.success) {
            this.todoList = res.body.content
            this.total = res.body.total
            this.loading = false
            this.todoList.forEach((item, index) => {
              item.releaseTime = this.formatTime(item.releaseTime)
            })
          } else {
            this.loading = false
          }
        })
        .catch(_ => {
          this.loading = false
        })
    }
  }
}
</script>

<style lang="less" scoped>
#helpdocument {
  background: #fff;
  .content-box {
    width: 90%;
    justify-content: space-around;
    margin-top: 38px;
    display: flex;
    margin: 20px auto;
    .informationMenu {
      width: 22%;
    }
    .informationContent {
      width: 78%;
    }
  }
  /deep/.el-input-group__append {
    background: #1890ff;
    color: #fff;
  }
  /deep/.el-input__inner {
    margin: 1px 0;
  }
  .pagination-box {
    text-align: end;
    padding: 24px;
    background: #fff;
  }
}
</style>
