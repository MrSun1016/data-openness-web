<template>
  <div>
    <el-card class="cardhei">
      <div class="title"><span>《</span>{{ questionItem.catalogName }}<span>》数据纠错</span></div>
      <el-divider></el-divider>
      <div class="divbody">
        <el-form
          ref="form"
          :model="form"
          :rules="rules"
          label-position="left"
          size="small"
          v-loading="drawerLoading"
          label-width="120px"
        >
          <p>
            <span class="itme-title">纠错详情： </span><span>{{ questionItem.correctionContent || '-' }}</span>
          </p>
          <p>
            <span class="itme-title">附件下载：</span><span>
               <template v-for="(item) in uploadFile">
                  <el-button type="text" @click="downLoad(item)">{{
                    item || '无'
                  }}</el-button>
                </template>
            </span>
          </p>
        </el-form>
        <div class="butPosition">
          <el-button size="small" @click="cancelNewData">取消</el-button>
        </div>
      </div>
    </el-card>
  </div>
</template>

<script>
import { getCorrectionById } from '@/api/api'
import { download } from '@/api/manage'
import { mapState } from 'vuex'
import { MessageBox, Message } from 'element-ui'
export default {
  name: 'CorrectionDetail',
  data() {
    return {
      questionItem: {
        catalogName: '',
        correctionContent: '',
        attachmentDownload: [],
        createdTime: ''
      },
      uploadFile: [],
    }
  },
  mounted() {},
  methods: {
        // 文件下载
    downLoad(fileName) {
      download({ fileName })
        .then((res) => {
          const blob = new Blob([res], {
            type: 'application/json;charset=UTF-8',
          }) // res就是接口返回的文件流
          const link = document.createElement('a') // 创建a标签
          const objectUrl = window.URL.createObjectURL(blob)
          link.href = objectUrl
          link.download = fileName
          link.click()
          window.URL.revokeObjectURL(objectUrl) // 释放内存
        })
        .catch((error) => {
          this.$message.warning('模板导出失败')
        })
    },
    getContent(row) {
      getCorrectionById(row.id).then(res => {
        if (res.success) {
          this.questionItem = res.body
          this.uploadFile = res.body.attachmentDownload.split(",")
          this.questionItem.createdTime = this.formatTime(this.questionItem.createdTime)
        } else {
          Message.error(res.message)
        }
      })
    },
    closeMessage() {
      this.questionItem = {}
      this.$emit('update:visible', false)
    },
    cancelNewData(){
       this.$emit('update:visible', false)
    }
  }
}
</script>

<style lang="less" scoped>
/deep/.title {
    height: 60px;
    line-height: 60px;
    text_align:center;
    font_size:18px;
    font-weight:bold;
  }
.itme-cont{
  width: 100%;
  height: 50%;
}
/deep/.el-divider--horizontal {
  background-color: #1890ff;
}
/deep/.el-card__body {
  width: 100%;
  height: 100%;
}
.cardhei {
  height: 100%;
  .divbody {
    width: 100%;
    height: 100%;
    position: relative;
    .butPosition {
      display: flex;
      justify-content: flex-end;
    }
  }
}
.checkboxFlex {
  display: flex;
  flex-direction: column;
}
.frombigweiht {
  width: 65vw;
}
.itme-title {
  padding: 32px;
  width: 160px;
  height: 45px;
  display: inline-block;
}
</style>
