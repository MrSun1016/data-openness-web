<template>
  <div id="ServiceInquire">
    <el-form class="inquire-box" :model="serviceList" label-position="left" inline @keyup.enter.native="serviceSearch">
      <el-row :gutter="20">
        <el-col :span="8">
          <el-form-item label="实施清单编码："  label-width="130px" >
            <el-input  v-model="serviceList.implementationCode" size="small" clearable placeholder="请输入"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="实施清单名称："  label-width="130px" >
            <el-input  v-model="serviceList.implementationName" size="small" clearable placeholder="请输入"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="8" style="text-align: right;">
          <el-form-item label="目录名称：">
            <el-input  v-model="serviceList.catalogName" size="small" clearable placeholder="请输入"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="业务办理项名称："  label-width="130px" >
            <el-input  v-model="serviceList.businessName" size="small" clearable placeholder="请输入"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="业务办理项编码："  label-width="130px" >
            <el-input   v-model="serviceList.businessCode" size="small" clearable placeholder="请输入"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="8" style="text-align: right;">
          <el-form-item class="form-items">
            <el-button size="small" type="primary" @click="serviceSearch">搜 索</el-button>
            <el-button size="small" @click="handleReset">重 置</el-button>
          </el-form-item>
        </el-col>
      </el-row>
    </el-form>
  </div>
</template>

<script>
export default {
  name: 'ServiceInquire',
  data() {
    return {
      inquireFormData: {},
      serviceList: {
        implementationCode: '',
        implementationName: '',
        businessCode: '',
        businessName: '',
        catalogName:'',
      }
    }
  },
  mounted() {},
  methods: {
    // 重置
    handleReset(){
      this.$bus.$emit('onReset')
      this.serviceList = {}
    },
    serviceSearch(){
      this.$emit('serviceValue', this.serviceList)
    }
  }
}
</script>

<style lang="less" scoped>
#ServiceInquire {
  .inquire-box {
    padding: 32px;
  }
  /deep/.el-form-item {
    margin-right: 0px !important;
  }
}
</style>
