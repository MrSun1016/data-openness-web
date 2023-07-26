<template>
  <div id="basicMsg">
    <!-- 基本信息 -->
    <div style="padding: 40px">
      <el-form ref="form" :model="form" :rules="rules" label-width="100px">
        <el-form-item label="用户名：" prop="name">
          <el-input v-model="form.name" size="mini" disabled></el-input>
        </el-form-item>
        <el-form-item label="用户级别：">
          <div>已实名认证</div>
        </el-form-item>
        <el-form-item label="姓名：" prop="name1">
          <el-input v-model="form.name1" size="mini" disabled></el-input>
        </el-form-item>
        <el-form-item label="身份证号码：">
          <div>420902********6432</div>
        </el-form-item>
        <el-form-item label="手机号码：">
          <div>13871852655</div>
        </el-form-item>
        <el-form-item label="电子邮箱：">
          <div>暂无</div>
        </el-form-item>
      </el-form>
    </div>
    <!-- 应用信息 -->
    <div class="usemsg-title">应用信息</div>
    <div class="usemsg-box" style="padding: 40px">
      <div class="usemsg-content">
        <el-form ref="form" :model="form" label-width="100px">
          <el-form-item label="应用名称：">
            <div></div>
          </el-form-item>
          <el-form-item label="应用描述：">
            <div></div>
          </el-form-item>
          <el-form-item label="paasld:">
            <div></div>
          </el-form-item>
          <el-form-item label="token：">
            <div>暂无</div>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
import TrendVue from '../../../components/Trend/Trend.vue'
export default {
  name: 'basicMsg',
  data() {
    return {
      form: {
        name: '',
        name1: '',
      },
      rules: {
        name: [{ required: true }],
        name1: [{ required: TrendVue }],
      },
    }
  },
}
</script>

<style lang="less" scoped>
#basicMsg {
  height: calc(100vh - 105px);
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: auto;
  margin: 0 24px;
  background-color: #edf1f6;
  .usemsg-box {
  }
  .usemsg-title {
    color: #56bcea;
    font-size: 20px;
    padding: 0 0 15px 15px;
    border-bottom: 1px solid #e8e8e8;
  }
}
</style>