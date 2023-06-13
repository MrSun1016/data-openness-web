<template>
  <div>
    <el-card>
      <div class="closeicon">
        <i class="el-icon-close icon-font" @click="closeMessage"></i>
      </div>
      <div class="body-div">
        <h2 class="title-h2">问题详情</h2>
        <div class="Information-bar">
          <div class="information">问题标题:{{ questionItem.title }}</div>
          <div class="information">提交时间:{{ questionItem.createdTime }}</div>
          <div class="information">附件下载:{{ questionItem.attachmentDownload }}</div>
          <div class="information">问题内容：{{ questionItem.content }}</div>
        </div>
        <div class="content-div">
          <div class="content-p2" v-html="questionItem.content">
          </div>
        </div>
      </div>
    </el-card>
  </div>
</template>
<script>
import { getQuestionById } from '@/api/api'
import { MessageBox, Message } from 'element-ui'
export default {
  name: 'CommonQuestionDetail',
  data() {
    return {
      questionItem: {
        title: '',
        content: '',
        attachmentDownload: '',
        createdTime: ''
      },
    }
  },
  mounted() {},
  methods: {
    getContent(row) {
      let record = row.id
      getQuestionById(record).then(res => {
        if (res.success) {
          this.questionItem = res.result
          this.questionItem.createdTime = this.formatTime(this.questionItem.createdTime)
        } else {
          Message.error(res.message)
        }
      })
    },
    announcementpreview(form, val) {
      this.questionItem = form
      this.valName = val
    },
    closeMessage() {
      this.questionItem = {}
      this.$emit('update:visible', false)
    }
  }
}
</script>

<style lang="less" scoped>
.closeicon {
  display: flex;
  flex-direction: row-reverse;
  color: rgb(112, 112, 112);
  .icon-font {
    font-size: 25px;
    cursor: pointer;
  }
}
.body-div {
  .title-h2 {
    text-align: center;
  }
  .Information-bar {
    padding: 10px 0;
    margin: 10px 0 20px 0;
    background-color: #f2f2f2;
    display: flex;
    justify-content: space-evenly;
  }
}
.content-div {
  .content-p {
    white-space: pre-line;
  }
  .content-p2 {
    white-space: pre-line;
    margin-top: 10px;
  }
}
</style>
