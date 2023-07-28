<template>
  <div id="personalAdd">
    <div class="info">基本信息</div>
    <el-form :model="formInline" class="demo-form-inline" :rules="rules">
      <div class="item-box">
        <el-form-item label="应用名称" prop="user">
          <el-input v-model="formInline.user" placeholder="审批人" size="small"></el-input>
        </el-form-item>
        <el-form-item label="应用类型" prop="region">
          <el-select v-model="formInline.region" placeholder="请选择" size="small">
            <el-option label="类型一" value="shanghai"></el-option>
            <el-option label="类型二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
      </div>
      <div class="item-box">
        <el-form-item label="所属领域" prop="date1">
          <el-select v-model="formInline.region" placeholder="请选择" size="small">
            <el-option label="领域一" value="shanghai"></el-option>
            <el-option label="领域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="关联数据">
          <el-select v-model="formInline.region" placeholder="请选择" size="small">
            <el-option label="数据一" value="shanghai"></el-option>
            <el-option label="数据二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
      </div>
      <el-form-item label="应用介绍" prop="date2">
        <el-input
          v-model="formInline.user"
          type="textarea"
          minlength="20"
          show-word-limit
          placeholder="请输入（20字以上）"
        ></el-input>
      </el-form-item>
      <el-form-item label="logo图片" prop="date3">
        <el-upload action="#" list-type="picture-card" :auto-upload="false" :limit="1">
          <i slot="default" class="el-icon-plus"></i>
          <div slot="file" slot-scope="{ file }">
            <img class="el-upload-list__item-thumbnail" :src="file.url" alt="" />
            <span class="el-upload-list__item-actions">
              <span class="el-upload-list__item-preview" @click="handlePictureCardPreview(file)">
                <i class="el-icon-zoom-in"></i>
              </span>
              <span v-if="!disabled" class="el-upload-list__item-delete" @click="handleDownload(file)">
                <i class="el-icon-download"></i>
              </span>
              <span v-if="!disabled" class="el-upload-list__item-delete" @click="handleRemove(file)">
                <i class="el-icon-delete"></i>
              </span>
            </span>
          </div>
        </el-upload>
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt="" />
        </el-dialog>
      </el-form-item>
      <el-form-item label="应用简介图片">
        <el-upload action="#" list-type="picture-card" :auto-upload="false" :limit="3">
          <i slot="default" class="el-icon-plus"></i>
          <div slot="file" slot-scope="{ file }">
            <img class="el-upload-list__item-thumbnail" :src="file.url" alt="" />
            <span class="el-upload-list__item-actions">
              <span class="el-upload-list__item-preview" @click="handlePictureCardPreview(file)">
                <i class="el-icon-zoom-in"></i>
              </span>
              <span v-if="!disabled" class="el-upload-list__item-delete" @click="handleDownload(file)">
                <i class="el-icon-download"></i>
              </span>
              <span v-if="!disabled" class="el-upload-list__item-delete" @click="handleRemove(file)">
                <i class="el-icon-delete"></i>
              </span>
            </span>
          </div>
        </el-upload>
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt="" />
        </el-dialog>
      </el-form-item>
    </el-form>
    <div style="text-align: right">
      <el-button type="primary" size="small">保 存</el-button>
      <el-button size="small">提交审核</el-button>
      <el-button size="small" @click="handleCancel">取 消</el-button>
    </div>
  </div>
 
</template>

<script>
export default {
  name: 'personalAdd',
  data() {
    return {
      formInline: {
        user: '',
        region: '',
      },
      dialogImageUrl: '',
      dialogVisible: false,
      disabled: false,
      rules: {
        user: [{ required: true, message: '请输入应用名称', trigger: 'blur' }],
        region: [{ required: true, message: '请选择应用类型', trigger: 'change' }],
        date1: [{ required: true, message: '请选择所属领域', trigger: 'change' }],
        date2: [{ required: true, message: '请输入应用介绍', trigger: 'blur' }],
        date3: [{ required: true, message: '请上传logo图片', trigger: 'blur' }],
      },
    }
  },
  methods: {
    //取消
    handleCancel() {
      this.$router.back()
    },
    handleRemove(file) {
      console.log(file)
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url
      this.dialogVisible = true
    },
    handleDownload(file) {
      console.log(file)
    },
  },
}
</script>

<style lang="less" scoped>
#personalAdd {
  padding: 40px;
  margin: 20px;
  background: #edf1f6;
  .info {
    font-weight: bold;
  }
  .item-box {
    display: flex;
    justify-content: space-between;
  }
}
/deep/.el-input__inner {
  width: 330px;
}
/deep/ .el-textarea__inner {
  width: 80%;
}
/deep/ .el-form-item__content {
  display: flex;
}
</style>