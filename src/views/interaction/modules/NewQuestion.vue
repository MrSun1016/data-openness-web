<template>
  <div>
    <el-card class="cardpaddin" v-show="!isshowMessage" v-loading="drawerLoading">
      <div class="titleflex">
        <p class="titlep">新增常见问题</p>
      </div>
      <el-divider></el-divider>
      <div class="bodydiv">
        <el-form ref="form" :model="form" :rules="rules" label-position="left" label-width="80px">
          <el-form-item label="标题" prop="title">
            <el-input size="small" v-model="form.title"></el-input>
          </el-form-item>
          <el-form-item label="内容" prop="content">
            <el-input
              type="textarea"
              :rows="3"
              maxlength="300"
              height="240"
              v-model="form.content"
              placeholder="请输入（20字以上）"
              class="frombigweiht"
            ></el-input>
          </el-form-item>
          <el-form-item label="附件上传" prop="file">
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
    <CommonQuestionDetail ref="content" v-show="isshowMessage" :visible.sync="isshowMessage"></CommonQuestionDetail>
  </div>
</template>
<script>
import { MessageBox, Message } from 'element-ui'
import { postQuestionAdd, getQuestionById, postQuestionEdit } from '@/api/api'
import CommonQuestionDetail from './CommonQuestionDetail'
import { indexOf } from 'lodash'
export default {
  name: 'NewQuestion',
  components: { CommonQuestionDetail },
  data() {
    return {
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
        title: '',
        content: '',
        attachmentDownload: ''
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
        title: [{ required: true, message: '请输入信息资源名称', trigger: 'blur' }],
        content: [{ required: true, message: '请输入公告内容', trigger: 'blur' }],
      },
      loadSave: false,
      loadhandle: false,
      // 加载
      drawerLoading: false,
    }
  },
  mounted() {
  },
  methods: {
    handleChange(file, fileList) {
      let resFileName = file.name.split('.')
      this.fileName.fileName = resFileName[0]
    },
    // 复制/在编辑
    duplicateAnnounce(row) {
      this.drawerLoading = true
      this.duplicateId = row.id
      getQuestionById(row.id).then((res) => {
        if (res.success) {
          this.form = res.body
          if (res.body.attachmentDownload != null && res.body.attachmentDownload != '') {
            let attachmentDownload = res.body.attachmentDownload
            // if (attachmentDownload != -1) {
            //   let resUploadFileName = JSON.parse(res.body.attachmentDownload)
            //   resUploadFileName.forEach((v) => {
            //     this.fileList.push({
            //       name: v,
            //     })
            //   })
            // } else {
              this.fileList.push({
                name: res.body.attachmentDownload,
              })
            // }
            this.drawerLoading = false
          } else {
            Message.error(res.message)
            this.drawerLoading = false
          }
        }
      })
    },
    // 保存
    preserve() {
      if (this.form.title == '') {
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
        if(this.form.id == ''){//保存
          postQuestionAdd(this.form).then((res) => {
            if (res.success) {
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
              Message.error(res.message)
            }
          })
        }else{//修改
          postQuestionEdit(this.form).then((res) => {
          if (res.success) {
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
            Message.error(res.message)
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
        title: '',
        content: '',
        attachmentDownload: ''
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
      this.form.attachmentDownload = file.name 
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
// /deep/.el-card__body {
//   padding: 0;
// }
.titleflex {
  margin: 20px 20px 15px 20px;
  display: flex;
  justify-content: space-between;
  .titlep {
    font-size: 16px;
    height: 35px;
    font-weight: 600;
    line-height: 35px;
    margin: 0;
  }
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
