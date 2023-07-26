<template>
  <div id="FeedbackDetails">
    <div class="detailstitle">
      <div class="header">
        <i class="el-icon-arrow-left" @click="closeFeedback"></i>
        问题反馈
      </div>
      <div>2023/05/06 11:23</div>
    </div>
    <div class="detailsList">
      <div class="detailshrader">
        <div class="title">反馈标题：</div>
        <div>提交按钮功能缺陷</div>
      </div>
      <div class="detailshrader">
        <div class="title">反馈类型：</div>
        <div>功能缺陷</div>
      </div>
      <div class="detailshrader">
        <div class="title">反馈内容：</div>
        <div>操作按钮功能缺陷</div>
      </div>
    </div>
    <div class="detailstitle">
      <div class="header2">
        <!-- <i class="el-icon-arrow-left"></i> -->
        问题回复
      </div>
      <div>2023/05/06 11:23</div>
    </div>
    <div class="detailsList">
      <div class="detailshrader">
        <div class="title">反馈标题：</div>
        <div>提交按钮功能缺陷</div>
      </div>
      <div class="detailshrader">
        <div class="title">反馈类型：</div>
        <div>功能缺陷</div>
      </div>
      <div class="detailshrader">
        <div class="title">反馈内容：</div>
        <div>操作按钮功能缺陷</div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'FeedbackDetails',
  data() {
    return {}
  },
  methods:{
      closeFeedback(){
      this.$emit('closeFeedback', false)
      }
  }
}
</script>

<style lang="less" scoped>
#FeedbackDetails {
  width: 100%;
  background-color: #fafafc;
  .detailstitle {
    padding: 20px 0;
    margin: 0 30px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid #edf1f6;
    .header {
      color: #191c20;
      font-size: 17px;
    }
    .header2 {
      color: #191c20;
      font-size: 17px;
      margin-left: 15px;
    }
  }
  .detailsList {
    padding: 15px 0 50px 0;
    margin: 0 60px;
    .detailshrader {
      //   color: #191c20;
      margin: 15px 0;
      display: flex;
      align-items: center;
      .title {
        width: 100px;
        color: #191c20;
      }
    }
  }
}
</style>
