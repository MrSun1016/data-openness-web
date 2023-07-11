<template>
  <!-- 数据编目--编目列表-新增/编辑 -->
  <div>
    <el-card class="cardpad" v-loading="loading">
      <p class="jibentop">基本信息</p>
      <el-divider class="pdivider"></el-divider>
      <el-form
        :inline="true"
        label-width="140px"
        size="small"
        :label-position="labelPosition"
        :model="formLabelAlign"
        :rules="formLabelAlignRules"
        ref="formLabelAlign"
        class="frommaglef"
      >
        <div class="formdivflex">
          <el-form-item label="应用名称" key="1" class="fromtopmag" prop="applyName">
            <el-input v-model.trim="formLabelAlign.applyName" placeholder="请输入" class="fromweiht"></el-input>
          </el-form-item>
          <el-form-item label="应用类型" key="3" class="fromtopmag" prop="applicationType">
            <el-select v-model="formLabelAlign.applicationType" placeholder="请选择" class="fromweiht">
              <el-option
                :label="openitem.text"
                :value="openitem.value"
                v-for="(openitem, index) in datefromList.applicationType"
                :key="openitem.value"
              ></el-option>
            </el-select>
          </el-form-item>
          <div style="width:30px"></div>
        </div>

        <div class="formdivflex">
          <el-form-item label="所属领域" key="11" class="fromtopmag" prop="keyAreasType">
            <el-select v-model="formLabelAlign.keyAreasType" filterable placeholder="请选择" class="fromweiht">
              <el-option
                :label="areas.text"
                :value="areas.value"
                v-for="(areas, index) in datefromList.keyareastype"
                :key="areas.value"
              ></el-option>
            </el-select>
          </el-form-item>

          <el-form-item label="关联数据" key="4" class="fromtopmag" prop="associatedData">
            <el-select v-model="formLabelAlign.associatedData" placeholder="请选择" class="fromweiht">
              <el-option
                :label="source.appName"
                :value="source.appName"
                v-for="(source, index) in associatedList"
                :key="source.id"
              ></el-option>
            </el-select>
          </el-form-item>
          <div style="width:30px"></div>
        </div>

        <el-form-item label="应用介绍" key="14" class="fromtopmag" prop="applyIntroduce">
          <el-input
            type="textarea"
            :rows="3"
            maxlength="300"
            v-model="formLabelAlign.applyIntroduce"
            placeholder="请输入（20字以上）"
            class="frombigweiht"
          ></el-input>
        </el-form-item>

        <el-form-item label="logo图片" key="14" class="fromtopmag">
          
          <el-upload
            action="/dtwave-om/api/file/wangeditor/upload"
            list-type="picture-card"
            :on-success="(response, file, fileList) => {handleSuccess(response, file, fileList)}"
            :file-list="fileList"
            accept="image/jpg,image/jpeg,image/png" 
            :before-upload="handleBeforeUpload"
            ref="upload"
            :auto-upload="true"
          >
            <i slot="default" class="el-icon-plus"></i>
            <div slot="file" slot-scope="{file}">
              <img class="el-upload-list__item-thumbnail" :src="file.url" alt />
              <span class="el-upload-list__item-actions">
                <span
                  class="el-upload-list__item-preview"
                  @click="handlePictureCardPreview(file)"
                >
                  <i class="el-icon-zoom-in"></i>
                </span>
                <!-- <span
                  v-if="!disabled"
                  class="el-upload-list__item-delete"
                  @click="handleDownload(file)"
                >
                  <i class="el-icon-download"></i>
                </span> -->
                <span
                  v-if="!disabled"
                  class="el-upload-list__item-delete"
                  @click="handleRemove(file)"
                >
                  <i class="el-icon-delete"></i>
                </span>
              </span>
            </div>
          </el-upload>
          <!-- 图片预览 -->
          <el-dialog :visible.sync="dialogVisible">
            <img width="100%" :src="dialogImageUrl" alt />
          </el-dialog>
        </el-form-item>

        <div>
          <div style="display: inline-block; width: 140px;">
            <el-form-item label="应用简介图片" key="14">
            </el-form-item>
          </div>
          <div style="display: inline-block;" class="upload">
            <el-upload
              action="/dtwave-om/api/file/wangeditor/upload"
              list-type="picture-card"
              :on-success="(response, file, fileList) => {handleSuccess(response, file, fileList)}"
              :file-list="fileList"
              accept="image/jpg,image/jpeg,image/png" 
              :before-upload="handleBeforeUpload"
              ref="upload"
              :auto-upload="true"
            >
              <i slot="default" class="el-icon-plus" style="margin-top: 180px;"></i>
              <div slot="file" slot-scope="{file}" style="height: 150%;">
                <img class="el-upload-list__item-thumbnail" :src="file.url" alt />
                <span class="el-upload-list__item-actions">
                  <span
                    class="el-upload-list__item-preview"
                    @click="handlePictureCardPreview(file)"
                  >
                    <i class="el-icon-zoom-in"></i>
                  </span>
                  <!-- <span
                    v-if="!disabled"
                    class="el-upload-list__item-delete"
                    @click="handleDownload(file)"
                  >
                    <i class="el-icon-download"></i>
                  </span> -->
                  <span
                    v-if="!disabled"
                    class="el-upload-list__item-delete"
                    @click="handleRemove(file)"
                  >
                    <i class="el-icon-delete"></i>
                  </span>
                </span>
              </div>
            </el-upload>
            <!-- 图片预览 -->
            <el-dialog :visible.sync="dialogVisible">
              <img width="100%" :src="dialogImageUrl" alt />
            </el-dialog>
          </div>
        </div>
      </el-form>

    </el-card>
    
  </div>
</template>

<script>
import { mapState } from 'vuex'
import {
  achievementPage,
  getAchievementInfo,
  achievementSave,
  queryAppName,
} from '@/api/api'
import { MessageBox, Message } from 'element-ui'
export default {
  name: 'AchievementNewData',
  components: {},
  data() {
    return {
      loading: false,
      loadSave: false,
      loadSubmit: false,
      isShowDisabled: false,
      cancelAddServ: false,
      isShowSave: false,// 保存
      labelPosition: 'left',
      associatedList: [],
      // 数据字典
      dataDictionary: {
        applicationType: 'application_type',// 开放类型
        keyareastype: 'key_areas_type',// 所属领域
      },
      formLabelAlign: {
        applyName: '', //应用名称
        catalogEn: '', //应用类型
        associatedData: '', //关联数据
        keyAreasType: '', //所属领域
        applyIntroduce: '', //应用介绍
      },
      // 表单校验
      formLabelAlignRules: {
        applyName: [{ required: true, message: '请输入应用名称', trigger: 'blur' }],
        applicationType: [{ required: true, validator: blurEn, message: '请输入应用类型', trigger: 'blur' }],
        associatedData: [{ required: true, message: '请选择关联数据', trigger: 'change' }], //调试注释
        keyAreasType: [{ required: true, message: '请选择所属领域', trigger: 'change' }],
        applyIntroduce: [
          { required: true, message: '请输入应用介绍', trigger: 'blur' },
          {
            min: 20,
            message: '请输入20字以上',
            trigger: 'blur'
          }
        ]
      },
      submitForm4: {
        fileIdList: []
      },
      uploadFileServiceUrl: '/dtwave-om/api/file/wangeditor/upload', // 文件上传的接口地址
    }
  },
  mounted() {
    // 新增页面选择框的数据-请求数据字典得到
    this.dictionaryList()
    // 关联数据
    // this.associatedDataList();
  },
  beforeDestroy() {
  },
  methods: {
    // 关联数据
    associatedDataList() {
      queryAppName().then(res => {
        this.associatedList = res.body
      })
    },
    // 新增页面选择框的数据-请求数据字典得到
    dictionaryList() {
      let {
        applicationType,
        keyareastype,
      } = this.dataDictionary
      // 应用类型
      this.$store.dispatch('getApplicationType', applicationType)
      // 所属领域
      this.$store.dispatch('getKeyareastypeList', keyareastype)
    },
    // 取消保存
    cancelSave() {
      this.isShowSave = false
      this.loadSave = false
    },
    // 点击保存发送请求
    save() {
      this.loadSave = true
      

      this.isShowSave = false
      this.loadSave = false
    },

    // 点击提交审核
    toShenhe() {
      this.isExamineDialogVisible = true
    },
    cancelAddSubmit() {
      this.isExamineDialogVisible = false
      this.loadSubmit = false
    },
    // 重置数据
    resetData() {
      this.formLabelAlign = {
        applyName: '', //应用名称
        catalogEn: '', //应用类型
        associatedData: '', //关联数据
        keyAreasType: '', //所属领域
        applyIntroduce: '', //应用介绍
      }
    },
    //应用成果
    handleSuccess(response, file, fileList){
      response.data.forEach((item,index) => {
        this.submitForm4.fileIdList.push(item.fileID)
      })
      
    },
    // 判断上传的是否为图片
    handleBeforeUpload(file) {
      var img = file.name.substring(file.name.lastIndexOf('.') + 1)
      const suffix = img === 'jpg'
      const suffix2 = img === 'png'
      const suffix3 = img === 'jpeg'
      // const isLt1M = file.size / 1024 / 1024 < 1;
      if (!suffix && !suffix2 && !suffix3) {
          this.$message.error("只能上传图片！");
          return false
      }
      // 可以限制图片的大小
      // if (!isLt1M) {
      //     this.$message.error('上传图片大小不能超过 1MB!');
      // }
      return suffix || suffix2 || suffix3
    },
    handleRemove(file,fileList) {
      const uploadFiles = this.$refs.upload.uploadFiles;
      for (let i in uploadFiles) {
        if (file.url === uploadFiles[i].url) {
          uploadFiles.splice(i, 1);
        }
      }
      // console.log(uploadFiles,'uploadFilesuploadFiles')
      this.getFileID(uploadFiles)
    },
    getFileID(fileList){
      let deleFileId = []
      fileList.forEach((item,index) => {
        console.log(index,item.response.data)
        item.response.data.forEach((item,index) => {
          // console.log(index,item)
          deleFileId.push(item.fileID)
          
        })
      })
      this.submitForm4.fileIdList = deleFileId
 
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },


  },
  computed: {
    ...mapState({
      datefromList: state => state.indatafrom.datefromList
    })
  }
}
</script>

<style lang="less" scoped>
/deep/.el-form-item__label:before {
  content: '*';
  color: #f56c6c;
  margin-right: 5px;
  font-weight: bold;
}

/deep/.el-form-item__label:before {
  content: '';
  margin-right: 10px;
  font-weight: bold;
}
.dataLis-demo {
  position: relative;
}
.demo:before {
  position: absolute;
  top: 2px;
  left: -12px;
  content: '*';
  color: #f56c6c;
  font-weight: bold;
}
/deep/.el-table .cell {
  padding-left: 15px;
  padding-right: 15px;
}
.formdivflex {
  display: flex;
  justify-content: space-between;
}
.el-select {
  /deep/.el-input {
    display: flex;
  }
}
/deep/.el-icon-arrow-up:before {
  content: '\e78f';
  color: #666666;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
h3 {
  margin: 20px 0 0 0;
  font-size: 15px;
  font-weight: 900;
}
.topfont {
  width: 43px;
  height: 29px;
  font-size: 21px;
}
.cardpad {
  padding-left: 20px;
  width: 100%;
}
.cardpadmargtop {
  margin-top: 24px;
  padding-left: 20px;
  width: 100%;
}
.frommaglef {
  font-size: 14px;
  width: 100%;
}
.fromtopmag {
  margin-top: 8px;
}
.fromweiht {
  width: 13vw;
}
.updatewei {
  width: 40%;
}
.timewei {
  width: 60%;
}
.fromweihtlong {
  width: 400px;
}
.frommagin {
  margin-right: 280px;
}
.frombigmagin {
  margin-right: 480px;
}
.frommaginmini {
  margin-right: 150px;
}
.frombigweiht {
  width: 65vw;
}
.fromsmowit {
  width: 13vw;
}
.soemagin {
  margin-right: 80px;
}
.frombiggmagin {
  margin-right: 800px;
}
.pcolorred {
  margin-left: 20px;
  color: red;
}
.pilangflex {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 20px 0 25px 0;
}
.pilangflexnew {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 10px 0 25px 0;
}
.spanfont {
  font-size: 16px;
}
// 接口
.httpweihei {
  width: 100%;
  height: 32px;
  border: 1px solid #1890ff;
  border-radius: 2%;
  background-color: #eff7ff;
}

.butflexcz {
  display: flex;
}
.butczwh {
  width: 100px;
  height: 35px;
  text-align: center;
  line-height: 35px;
  font-size: 14px;
  padding: 0;
  margin-right: 20px;
}

.wubut {
  position: absolute;
  top: 36px;
  left: 20px;
  width: 100px;
  z-index: 1;
  background-color: white;
}
.wubutwei {
  width: 120px;
  height: 32px;
  margin-left: 15px;
}
.imaginle {
  margin-left: 10px;
}
.footweiht {
  width: 100%;
}
.footmag {
  margin-top: 42px;
}
h3 {
  height: 27px;
  font-size: 17px;
  font-weight: 500;
  color: #333333;
  line-height: 27px;
}
.twofromflex {
  position: relative;
}
.towfrompos {
  position: absolute;
  top: -3px;
  left: 500px;
}
.fromgeshimag {
  margin-top: 30px;
}
.examinepcent {
  font-size: 16px;
  text-align: center;
  margin-top: 90px;
}
.fromiteminptwh {
  width: 550px;
  height: 313px;
}

.dialogmargdaoru {
  margin: 0 60px 0 60px;
  padding: 0;
}
.fromweihtbig {
  width: 500px;
}
.fromweihtheng {
  width: 100%;
}

.jibentop {
  font-size: 16px;
  font-weight: 600;
  color: #1890ff;
}
.jibentopsize {
  font-size: 14px;
  font-weight: 600;
  color: #1890ff;
}
.pdivider {
  margin: 15px 0;
}
::v-deep .el-divider {
  background-color: #1890ff;
}
.table-SelectedRow-bgcolor {
  td {
    background-color: #fa645f !important;
  }
}
.dialogcenter {
  text-align: center;
  line-height: 200px;
  font-size: 16px;
  color: #000;
}
/deep/.el-dialog__title {
  color: #1890ff;
}
/deep/.el-divider--horizontal {
  background-color: #1890ff;
  margin: 0;
}
/deep/.el-dialog__body {
  padding: 0 0 10px 0;
}
/deep/.el-icon-info {
  width: 26px;
  height: 26px;
  color: #1890ff;
}

.piliangfz {
  position: relative;
  width: 120px;
}
.piliangbut {
  position: relative;
  width: 120px;
  height: 32px;
  border-radius: 4px;
  border: 1px solid #1890ff;
  color: #1890ff;
  line-height: 32px;
  font-size: 14px;
  padding: 0 17px;
  i {
    margin-left: 15px;
    color: #1890ff;
  }
}
.buttblok {
  width: 120px;
  display: block;
  color: #1890ff;
}

.dialogmarg {
  margin: 32px 40px 10px 40px;
}
.centerupload {
  text-align: center;
}
.mbcolor {
  color: #1890ff;
}

.buttonflex {
  display: flex;
  justify-content: flex-end;
  .buttonmargin {
    margin-bottom: 20px;
    margin-right: 10px;
  }
  .buttonrigth {
    margin: 0 20px 20px 10px;
  }
}
.upload {
  /deep/ .el-upload--picture-card{
      width: 300px;
      height: 400px;
  }
  /deep/ .el-upload{
      width: 300px;
      height: 400px;
      line-height: 100px;
  }
  /deep/ .el-upload-list--picture-card .el-upload-list__item{
      width: 300px;
      height: 400px;
      line-height: 100px;
  }
  /deep/ .el-upload-list--picture-card .el-upload-list__item-thumbnail{
      width: 300px;
      height: 400px;
      line-height: 100px;
  }
  /deep/ .avatar{
      width: 300px;
      height: 400px;
  }
}


</style>
