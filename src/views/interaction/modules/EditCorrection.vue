<template>
  <div>
    <el-card class="cardpaddin" v-show="!isshowMessage" v-loading="drawerLoading">
      <div class="bodydiv">

        <el-form ref="form" :model="form" :rules="rules" label-position="left" label-width="80px">
          <div class="title">
           <span>《</span>{{ catalogNames }}<span>》数据纠错</span>
          </div>
          <el-form-item label="纠错内容" prop="correctionContent">
            <el-input
              type="textarea"
              :rows="3"
              maxlength="300"
              height="240"
              v-model="form.correctionContent"
              placeholder="请输入（20字以上）"
              class="frombigweiht"
            ></el-input>
          </el-form-item>
          <el-form-item label="上传附件" prop="file">
            <el-upload
              class="upload-demo"
              name="file"
              action="/dtwave-om/api/file/uploadManage"
              multiple
              :headers="headerObj"
              :data="fileName"
              :on-error="xiaHandleExceed"
              :on-success="fileSuccess"
              :on-change="handleChange"
              :file-list="fileList"
            >
              <el-button size="small" type="primary" style="marginright: 20px">上 传</el-button>
              <span slot="tip" class="el-upload__tip">支持图片\WORD\EXCEL\PDF\压缩包</span>
            </el-upload>
          </el-form-item>
        </el-form>
        <div class="newbutflex">
          <el-button type="primary" size="small" class="butfromccwei" @click="preserve">保 存</el-button>
          <el-button size="small" @click="closeNewsAdd" class="butfromccwei">取 消</el-button>
        </div>
      </div>

      <!-- 保存 -->
      <el-dialog title="提示" :visible.sync="isShowSave" width="30%">
        <el-divider></el-divider>
        <p class="dialogcenter" v-show="selectId == '0'">是否确认取消</p>
        <p class="dialogcenter" v-show="selectId == '1'">是否确认保存?</p>
        <span slot="footer" class="dialog-footer">
          <el-button @click="cancelSave" size="small" class="deletmarg">取 消</el-button>
          <el-button type="primary" size="small" @click="save">确 认</el-button>
        </span>
      </el-dialog>
    </el-card>
    <CorrectionDetail ref="content" v-show="isshowMessage" :visible.sync="isshowMessage"></CorrectionDetail>
  </div>
</template>
<script>
import { MessageBox, Message } from 'element-ui'
import { postCorrectionAdd, getCorrectionById, postCorrectionEdit,getaddNewget } from '@/api/api'
import CorrectionDetail from './CorrectionDetail'
import { indexOf } from 'lodash'
export default {
  name: 'EditCorrection',
  components: { CorrectionDetail },
  data() {
    return {
      catalogId: '',
      catalogNames: '',
      fileName: {
        fileName: '',
      },
      // 上传
      headerObj: {
        'X-Access-Token': JSON.parse(window.localStorage.getItem('pro__Access-Token')).value,
      },
      //   表单
      form: {
        id: '',
        catalogId: '',
        catalogName: '',
        correctionContent: '',
        attachmentDownload: [],
      },
      checkedCities: [],
      checkAll: false,
      isIndeterminate: false,
      //   上传
      file: '',
      filename: '',
      fileList: [],
      uploadFileName: [],
      expireTimeOPtion: {
        disabledDate(time) {
          return time.getTime() < Date.now() //如果没有后面的-8.64e7就是不可以选择今天的
        },
      },
      // 保存
      isShowSave: false,
      selectId: '',
      // 发布
      isShowpostNew: false,
      // 预览
      isshowMessage: false,
      // 复制
      duplicateId: '',
      rules: {
        catalogName: [{ required: true, message: '请输入信息资源名称', trigger: 'blur' }],
        correctionContent: [{ required: true, message: '请输入公告内容', trigger: 'blur' }],
      },
      loadSave: false,
      loadhandle: false,
      // 加载
      drawerLoading: false,
    }
  },
  mounted() {
    this.catalogId = this.$route.query.id
    if(this.catalogId != ''){
      getaddNewget(this.catalogId).then((res) => {
        if (res.success) {
          this.catalogNames = res.body.catalogName
        } else {
            Message.error(res.message)
        }
      })
    }
  },
  methods: {
    getaddCorrection(){
      getaddNewget(this.catalogId).then((res) => {
        if (res.success) {
          this.catalogNames = res.body.catalogName
        } else {
            Message.error(res.message)
        }
      })
      },
    handleChange(file, fileList) {
      let resFileName = file.name.split('.')
      this.fileName.fileName = resFileName[0]
    },
    // 编辑
    duplicateAnnounce(row) {
      this.drawerLoading = true
      this.duplicateId = row.id
      getCorrectionById(row.id).then((res) => {
        if (res.success) {
          this.form = res.body
          this.catalogNames = res.body.catalogName
          this.catalogId = res.body.catalogId
          if (res.body.attachmentDownload != null && res.body.attachmentDownload != '') {
            let attachmentDownload = res.body.attachmentDownload.split(",")
            if (attachmentDownload != '') {
              let resUploadFileName = attachmentDownload
              resUploadFileName.forEach((v) => {
                this.fileList.push({
                  name: v,
                })
              })
            } else {
              this.fileList.push({
                name: res.body.attachmentDownload,
              })
            }
            this.drawerLoading = false
          } else {
            Message.error(res.result.message)
            this.drawerLoading = false
          }
        }
      })
    },
    // 保存
    preserve() {
      this.form.catalogName =  this.catalogNames
      if (this.form.catalogName == '') {
        Message.error('请填写标题')
      } else {
        this.selectId = '1'
        this.isShowSave = true
      }
    },
    save() {
      if (this.selectId == '0') {
        this.cleanUp()
        this.isShowSave = false
        this.$emit('update:visible', false)
      } else {
        this.form.catalogId = this.catalogId
        this.form.catalogName =  this.catalogNames
        if(this.form.id == ''){//保存
          postCorrectionAdd(this.form).then((res) => {
            if (res.result.success) {
              Message({
                message: '保存成功！',
                type: 'success',
              })
              this.isShowSave = false
              this.cleanUp()
              this.$emit('update:visible', false)
              this.$emit('refresh')
            } else {
              this.isShowSave = false
              Message.error(res.result.message)
            }
          })
        }else{//修改
          postCorrectionEdit(this.form).then((res) => {
          if (res.result.success) {
            Message({
              message: res.message,
              type: 'success',
            })
            this.isShowpostNew = false
            this.cleanUp()
            this.$emit('update:visible', false)
            this.$emit('refresh')
          } else {
            this.isShowpostNew = false
            Message.error(res.result.message)
          }
          this.loadSave = false
        })
        }

      }
    },
    cancelNew() {
      this.isShowpostNew = false
    },

    // 取消保存
    cancelSave() {
      this.isShowSave = false
    },
    cleanUp() {
      this.form = {
        catalogId: '',
        catalogName: '',
        correctionContent: '',
        attachmentDownload: [],
      }
      this.duplicateId = ''
      this.fileList = []
    },
    // 取消
    closeNewsAdd() {
      this.selectId = '0'
      this.isShowSave = true
    },
    // 文件上传失败
    xiaHandleExceed(err, file, fileList) {
      Message.error(err)
    },
    // 文件上传成功
    fileSuccess(res, file, fileList) {
      if(this.form.attachmentDownload == ''){
          this.form.attachmentDownload = file.name
          this.form.attachmentDownload = file.name.split(" ")
      }else{
          if(this.form.attachmentDownload instanceof Array){
              this.form.attachmentDownload.push(file.name)
          }else{
              this.form.attachmentDownload = this.form.attachmentDownload.split(" ")
              this.form.attachmentDownload.push(file.name)
          }

      }
      this.fileList = fileList
      if (res.success) {
        Message.success('上传成功')
      } else {
        Message.error('上传失败')
      }
    }
  }
}
</script>

<style lang="less" scoped>
  /deep/.title {
    height: 60px;
    line-height: 60px;
    text_align:center;
    font_size:18px;
    font-weight:bold;
  }
/deep/.el-divider--horizontal {
  background-color: #1890ff;
}
.bodydiv {
  margin: 15px 20px 20px 20px;
}
.newbutflex {
  display: flex;
  flex-direction: row-reverse;
  .butfromccwei {
    margin-left: 15px;
  }
}
/deep/.el-dialog__body {
  padding: 0;
}
/deep/.el-dialog__title {
  color: #1890ff;
}
/deep/.el-divider--horizontal {
  margin: 0;
}
.dialogbody {
  display: flex;
  justify-content: center;
}
.datepicker {
  // width: 300px;
  margin: 70px 0;
}
.dialogcenter {
  text-align: center;
  line-height: 200px;
  font-size: 16px;
  color: #000;
}
.frombigweiht {
  width: 65vw;
}
</style>
