<template>
  <div id="ErrorCorrectionDetails">
    <!-- <div class="details"> -->
    <div class="details">
      <div class="header">
        制造业企业名单信息纠错
      </div>
      <i class="el-icon-close" @click="closeError"></i>
    </div>
    <div class="conten">
      <div class="leftmax">
        <div class="title">
          纠错详情：
        </div>
        <div class="text">
          科研项目的研究过程中，需要一个区域性的停车场空余车位信息，来训练和测试用于空余停车位数量预测的深度学习模型。
        </div>
      </div>
      <div class="time">2023/06/03 22:11:12</div>
    </div>
    <div class="conten2">
      <div class="leftmax">
        <div class="title">
          回复信息：
        </div>
        <div class="text">
          已公开 请查看链接 <span> https://data.wuhan.gov.cn/page/data/data_directory_details.html?cataId</span>
        </div>
      </div>
    </div>
    <div class="foot">
      <span class="time">2023/06/03 22:11:12</span>
      <span class="geju">孝感市住建局</span>
    </div>
    <!-- </div> -->
  </div>
</template>
<script>
export default {
  name: 'ErrorCorrectionDetails',
  data() {
    return {}
  },
  methods: {
    closeError() {
      this.$emit('errorCorrection', false)
    }
  }
}
</script>

<style lang="scss" scoped>
#ErrorCorrectionDetails {
  width: 100%;
  background-color: #fafafc;

  .details {
    font-size: 17px;
    color: #191c20;
    text-align: center;
    padding: 20px 20px 40px 20px;
    position: relative;
    .el-icon-close {
      position: absolute;
      font-size: 23px;
      top: 20px;
      right: 20px;
    }
  }
  .conten {
    padding: 0 0 40px 0;
    margin: 0 40px;
    border-bottom: 1px solid #edf1f6;
    display: flex;
    justify-content: space-between;
    .leftmax {
      width: 75%;
      display: flex;
      .title {
        width: 90px;
        color: #191c20;
      }
    }
  }
  .conten2 {
    padding: 0 0 40px 0;
    margin: 50px 40px 0 40px;
    // border-bottom: 1px solid #edf1f6;
    display: flex;
    justify-content: space-between;
    .leftmax {
      width: 75%;
      display: flex;
      .title {
        width: 90px;
        color: #191c20;
      }
      .text {
        span {
          color: #4888ff;
        }
      }
    }
  }
  .foot {
    margin: 0 40px;
    padding: 40px 0;
    display: flex;
    flex-direction: row-reverse;
    .geju {
      margin-right: 30px;
    }
  }
}
</style>
