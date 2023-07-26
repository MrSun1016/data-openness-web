<template>
  <div id="RequirementResponse">
    <div class="detailstitle">
      <div class="header">
        <i class="el-icon-arrow-left" @click="closeFeedback"></i>
        申请内容
      </div>
      <div>2023/05/06 11:23</div>
    </div>
    <div class="detailsList">
      <div class="detailshrader">
        <div class="title">需求标题：</div>
        <div>停车场空余车位信息申请公开</div>
      </div>
      <div class="detailshrader">
        <div class="title">数据用途：</div>
        <div>
          科研项目的研究过程中，需要一个区域性的停车场空余车位信息，来训练和测试用于空余停车位数量预测的深度学习模型。
        </div>
      </div>
      <div class="detailshrader">
        <div class="title">期望数据格式：</div>
        <div>
          <el-radio v-model="radio" label="1">API</el-radio>
          <el-radio v-model="radio" label="2">excel</el-radio>
        </div>
      </div>
    </div>
    <div class="detailstitle">
      <div class="header2">
        <!-- <i class="el-icon-arrow-left"></i> -->
        回复信息
      </div>
      <div>2023/05/06 11:23</div>
    </div>
    <div class="detailsList">
      <div class="detailshrader">
        <div class="title">回复：</div>
        <div>已公开 请查看链接 <span>https://data.wuhan.gov.cn/page/data/data_directory_details.html?cataId</span></div>
      </div>
      <div class="detailshrader">
        <div class="title">回复部门：</div>
        <div>孝感市住建局</div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'RequirementResponse',
  data() {
    return { radio: '1' }
  },
  methods: {
    closeFeedback() {
      this.$emit('closeRequirement', false)
    }
  }
}
</script>

<style lang="less" scoped>
#RequirementResponse {
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
      margin: 20px 0;
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
