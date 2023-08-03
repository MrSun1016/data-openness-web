<template>
  <el-card id="fileViewer">
    <div class="closeicon">
      <i class="el-icon-close icon-font" @click="closeMessage"></i>
    </div>
    <div class="body-div">
      <!-- <h2 class="title-h2">{{ fileViewerData.informationName }}</h2> -->
      <div class="Information-bar">
        <div class="information">发布时间：{{ fileViewerData.createdTime }}</div>
        <div class="information">信息来源：{{ fileViewerData.deptName }}</div>
        <div class="information">访问量：{{ fileViewerData.viewNum }}</div>
      </div>
      <div class="content-div">
        <!-- <div class="content-p">摘要：{{ contenItem.msgAbstract }}</div> -->
        <div class="content-p2" v-html="fileViewerData.msgContent"></div>
      </div>
    </div>
  </el-card>
</template>
<script>
import { getInformationInfo } from '@/api/api'
import { MessageBox, Message } from 'element-ui'
export default {
  name: 'MessageContent',
  props: {
    fileViewerData: {
      Object,
      require: true,
    },
  },
  data() {
    return {}
  },
  mounted() {
    this.$bus.$on('handleCloseViewer', () => {
      this.closeMessage()
    })
  },
  methods: {
    closeMessage() {
      this.$emit('handleFileViewerClone', false)
    },
  },
}
</script>

<style lang="less" scoped>
#fileViewer {
  width: 100%;
  height: calc(100vh - 105px);
  // height: 100%;
  // display: flex;
  // flex-direction: column;
  overflow: scroll;
  // padding: 0 24px;
}
.closeicon {
  display: flex;
  flex-direction: row-reverse;
  // flex: 1;
  // width: 100%;
  color: rgb(112, 112, 112);
  .icon-font {
    font-size: 25px;
    cursor: pointer;
  }
}
.body-div {
  flex: 1;
  overflow: auto;
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
    /deep/ img {
      width: 100%;
      // height: 50px;
    }
  }
}
</style>
