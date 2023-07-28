<template>
  <div id="AskedQuestion">
    <div class="questionList">
      <div class="question">
        <span class="zimu">Q </span>
        <span>: 关于开放受限数据申请</span>
      </div>
      <div class="answer">
        <span class="zimu">A </span>
        <span>
          :
          申请受限开放数据的公民、法人和其他组织，应当具备安全保护、企业信用、数据存储处理等方面的资质和能力。申请需提交的材料包括公共数据开放申请表、公共数据开放需求清单、公共数据安全承诺书、公共数据安全等级保护证书、信用报告、以及其他相关资质和能力证明。材料模板可从“开发者中心-受限数据申请”路径进入后下载
        </span>
      </div>
    </div>
    <div class="questionList">
      <div class="question">
        <span class="zimu">Q </span>
        <span>: 关于开放受限数据申请</span>
      </div>
      <div class="answer">
        <span class="zimu">A </span>
        <span>
          :
          申请受限开放数据的公民、法人和其他组织，应当具备安全保护、企业信用、数据存储处理等方面的资质和能力。申请需提交的材料包括公共数据开放申请表、公共数据开放需求清单、公共数据安全承诺书、公共数据安全等级保护证书、信用报告、以及其他相关资质和能力证明。材料模板可从“开发者中心-受限数据申请”路径进入后下载
        </span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'AskedQuestion',
  data() {
    return {}
  }
}
</script>

<style lang="less" scoped>
#AskedQuestion {
  padding: 24px;
  width: 100%;
  margin: 0 20px;
  background-color: #fafafc;
  font-size: 17px;
  .questionList {
    padding-bottom: 24px;
    margin-bottom: 20px;
    border-bottom: 1px solid #edf1f6;
    .answer {
      margin-top: 20px;
      .zimu {
        color: #4888ff;
        font-weight: 500;
      }
    }
    .question {
      .zimu {
        color: #4888ff;
        font-weight: 500;
      }
    }
  }
}
</style>
