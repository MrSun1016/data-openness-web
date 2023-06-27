<template>
  <div>
    <el-card>
      <div class="closeicon">
        <i class="el-icon-close icon-font" @click="closeMessage"></i>
      </div>
      <div class="body-div">
        <h2 class="title-h2">{{ contenItem.titile }}</h2>
        <div class="Information-bar">
          <div class="information">发布时间：{{ contenItem.createdTime }}</div>
          <div class="information">信息来源：{{ contenItem.deptName }}</div>
          <div class="information">访问量：{{ contenItem.viewNum }}</div>
        </div>
        <div class="content-div">
          <!-- <div class="content-p">摘要：{{ contenItem.msgAbstract }}</div> -->
          <div class="content-p2" v-html="contenItem.msgContent">
          </div>
        </div>
      </div>
    </el-card>
  </div>
</template>
<script>
import { getInformationInfo } from '@/api/api'
import { MessageBox, Message } from 'element-ui'
export default {
  name: 'MessageContent',
  data() {
    return {
      contenItem: {
        titile: '',
        createdTime: '',
        deptName: '',
        viewNum: '',
        msgAbstract: '',
        msgContent: ''
      },
      valName: ''
    }
  },
  mounted() {},
  methods: {
    getContent(row) {
      let record = row.id
      getInformationInfo(record).then(res => {
        if (res.success) {
          this.contenItem = res.result
          this.contenItem.createdTime = this.formatTime(this.contenItem.createdTime)
        } else {
          Message.error(res.message)
        }
      })
    },
    announcementpreview(form, val) {
      this.contenItem = form
      // this.contenItem.titile = form.titile
      // this.contenItem.msgAbstract = form.msgAbstract
      this.valName = val
    },
    closeMessage() {
      this.contenItem = {}
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
