<template>
  <div>
    <el-card class="cardhei" v-show="!isshowMessage">
      <div class="titleflex">
        <p class="titlep">
          新增
        </p>
        <el-button type="primary" size="small" @click="preview">预 览</el-button>
      </div>
      <el-divider></el-divider>
      <div class="divbody">
        <el-form
          ref="form"
          :model="form"
          :rules="rules"
          label-position="left"
          size="small"
          v-loading="drawerLoading"
          label-width="120px"
        >
          <!-- <el-form-item label="资讯类型" prop="informationType">
            <el-select v-model="form.informationType" clearable placeholder="请选择" size="small">
              <el-option
                :label="manag.text"
                :value="manag.value"
                v-for="manag in datefromList.management"
                :key="manag.value"
              ></el-option>
            </el-select>
          </el-form-item> -->
          <el-form-item label="标题" prop="informationName">
            <el-input v-model="form.informationName"></el-input>
          </el-form-item>

          <!-- <el-form-item label="内容" prop="msgContent">
            <Wangeditor ref="wangeditor" v-model="form.msgContent"/>       
          </el-form-item> -->
          <el-form-item label="内容" prop="msgContent">
            <j-editor v-model="form.msgContent"></j-editor>
          </el-form-item>
          
          <el-form-item label="附件上传" prop="uploadFileName">
            <el-upload
              class="upload-demo"
              name="file"
              action="/dtwave-om/api/file/wangeditor/upload"
              multiple
              :headers="headerObj"
              :data="fileName"
              :on-error="xiaHandleExceed"
              :on-success="fileSuccess"
              :on-change="handleChange"
              :file-list="fileList"
            >
              <el-button size="small" type="primary" style="marginright: 20px">上 传</el-button>
              <span slot="tip" class="el-upload__tip">  支持图片\WORD\EXCEL\PDF\压缩包</span>
            </el-upload>
          </el-form-item>
          <!-- <el-form-item label="发布范围" prop="releaseScope">
            <template>
              <el-checkbox :indeterminate="isIndeterminate" v-model="checkAll" @change="handleCheckAllChange"
                >全选</el-checkbox
              >
              <div style="margin: 15px 0"></div>
              <el-checkbox-group class="checkboxFlex" v-model="form.releaseScope" @change="handleCheckedCitiesChange">
                <el-checkbox v-for="city in allRoleList" :label="city.id" :key="city.id">{{
                  city.roleName
                }}</el-checkbox>
              </el-checkbox-group>
            </template>
          </el-form-item> -->

          <el-form-item label="是否置顶">
            <el-switch v-model="form.isTop"></el-switch>
          </el-form-item>
        </el-form>
        <div class="butPosition">
          <el-button size="small" @click="cancelNewData">取消</el-button>
          <el-button type="primary" size="small" @click="onNewData(1)">保存</el-button>
          <el-button type="primary" size="small" @click="onNewData(3)">立即发布</el-button>
          <el-button type="primary" size="small" @click="subscribe">预约发布</el-button>
        </div>
       <!-- 预约发布弹窗 -->
      <el-dialog
        :close-on-click-modal="false"
        :modal-append-to-body="false"
        title="预约发布"
        :visible.sync="dialogVisible"
        width="30%"
      >
        <el-divider></el-divider>
        <div class="dialogbody">
          <el-form ref="dateForm" :model="dateForm" :rules="daterules" label-width="80px">
            <el-form-item label="预约时间" prop="appointTime" class="datepicker">
              <el-date-picker
                value-format="yyyy-MM-dd HH:mm:ss"
                :picker-options="expireTimeOPtion"
                @focus="focus"
                size="small"
                v-model="dateForm.appointTime"
                type="datetime"
                placeholder="请选择时间"
              >
              </el-date-picker>
            </el-form-item>
          </el-form>
        </div>
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible = false" size="small">取 消</el-button>
          <el-button type="primary" @click="onNewData(2)" size="small" :disabled="loadhandle" :loading="loadhandle">{{
            loadhandle ? '提交中...' : '确认'
          }}</el-button>
        </span>
      </el-dialog>
      
      </div>
    </el-card>
    <!-- <PreviewContent ref="content" v-show="isshowMessage" :visible.sync="isshowMessage"></PreviewContent> -->
    <MessageContent ref="content" v-show="isshowMessage" :visible.sync="isshowMessage"></MessageContent>
  </div>
</template>
<script>
import { getInformationAllRole, informationSave, informationUpdate, getInformationInfo } from '@/api/api'
import { MessageBox, Message } from 'element-ui'
// import Wangeditor from '@/views/InformationCenter/Wangeditor'
// import PreviewContent from './PreviewContent.vue'
import JEditor from '@/components/jeecg/JEditor'
import MessageContent from './MessageContent'
export default {
  name: 'LearningNewData',
  props: ['datefromList'],
  components: {
    // Wangeditor,
    // PreviewContent,
    JEditor, 
    MessageContent,
  },
  data() {
    return {
      fileName: {
        fileName: '',
      },
      headerObj: {
        'X-Access-Token': JSON.parse(window.localStorage.getItem('pro__Access-Token')).value,
      },
      form: {
        informationName: '',
        endDate: '',
        releaseState: '',//发布状态（1编辑中，2待发布，3已发布）
        informationType: '',//资讯类型（1:新闻动态 2:政策动态 3:公告通知 4:引导赋能	）
        id: 0,
        orderByField: '',
        pageNum: 0,
        pageSize: 0,
        releaseScope: [],
        rowStart: 0,
        startDate: '',
        uploadFileName: [],
        isTop: false,
        msgContent: '',
        title: '',
        appointTime: '',
      },
      reviseId: '',
      //文件  上传
      file: '',
      filename: '',
      fileList: [],
      errmesg: [],
      result: '',
      fileName1: '',
      //  发布范围
      allRoleList: [],
      checkedCities: [],
      checkAll: false,
      isIndeterminate: false,
      rules: {
        // informationType: [{ required: true, message: '请选择资讯类型', trigger: 'change' }],
        informationName: [{ required: true, message: '请输入标题', trigger: 'blur' }],
        // uploadFileName: [
        //   {
        //     required: true,
        //     message: '请至少选择上传一个文件',
        //     trigger: 'change',
        //   },
        // ],
        // releaseScope: [{ type: 'array', required: true, message: '请至少选择一个发布范围', trigger: 'change' }],
        msgContent: [{ required: true, message: '请输入正文内容', trigger: 'blur' }],
      },
      daterules: {
        appointTime: [{ required: true, message: '请选择预约发布时间', trigger: 'change' }],
      },
      drawerLoading: false,
      // 预览
      isshowMessage: false,
      dialogVisible: false,
      loadhandle: false,
      dateForm: { appointTime: '' },
    }
  },
  created() {},
  mounted() {},
  methods: {
    handleChange(file, fileList) {
      let resFileName = file.name.split('.')
      this.fileName.fileName = resFileName[0]
    },
    // 文件上传失败
    xiaHandleExceed(err, file, fileList) {
      Message.error(err)
    },
    // 文件上传成功
    fileSuccess(res, file, fileList) {
      try {
        this.form.uploadFileName.push(file.name)
      } catch (e) {
        this.form.uploadFileName = JSON.parse(this.form.uploadFileName)
        this.form.uploadFileName.push(file.name)
      }
      this.fileList = fileList
      //正文内容追加附件
      let url = window.location.origin+'/dtwave-om/api/file/information/download?fileName='+file.name;
      this.addAttachments(file.name,url);

      if (res.success) {
        Message.success(`${this.fileName.fileName}上传成功`)
      } else {
        Message.error(res.header.message)
      }
    },
    onNewData(param) {
      this.$refs['form'].validate((valid) => {
        if (valid) {
          if (this.form.isTop) {
            this.form.isTop = '1'
          } else {
            this.form.isTop = '0'
          }
          this.form.releaseState = param;//已发布
          if(param === 2){//预约发布
              this.$refs['dateForm'].validate((valid) => {
                if (valid) {
                  this.dialogVisible = false;
                  this.loadhandle = true;
                  this.form.appointTime = this.dateForm.appointTime;
                }
              })
          }
          if(this.loadhandle === true){
            if (this.reviseId === '') {
              informationSave(this.form).then((res) => {
                if (res.success) {
                  Message({
                    message: '发布成功！',
                    type: 'success',
                  })
                  this.$emit('isShowNewData', false)
                  this.clean()
                  // this.$refs.wangeditor.clean();
                } else {
                  Message.warning(res.message)
                }
              })
            } else {
              informationUpdate(this.form).then((res) => {
                if (res.success) {
                  Message({
                    message: '发布成功！',
                    type: 'success',
                  })
                  this.$emit('isShowNewData', false)
                  this.clean()
                  // this.$refs.wangeditor.clean();
                } else {
                  Message.warning(res.message)
                }
              })
            }
            this.reviseId = ''
          }
          this.loadhandle = false;
        }
      })
    },
    // 点击修改
    reviseDate(id) {
      // this.addAttachments();
      this.drawerLoading = true
      this.reviseId = id
      getInformationInfo(id).then((res) => {
        if (res.success) {
          this.form = res.result
          this.form.createdTime = this.formatTime(res.result.createdTime)
          // this.$refs.wangeditor.assignment(this.form.msgContent);

          if (res.result.uploadFileName != null && res.result.uploadFileName != '') {
            let UploadFileName = res.result.uploadFileName.indexOf('[')
            if (UploadFileName != -1) {
              let resUploadFileName = JSON.parse(res.result.uploadFileName)
              resUploadFileName.forEach((v) => {
                this.fileList.push({
                  name: v,
                })
              })
            } else {
              this.fileList.push({
                name: res.result.uploadFileName,
              })
            }
            this.drawerLoading = false
          } else {
            Message.error(res.message)
            this.drawerLoading = false
          }

          if (res.result.isTop == '1') {
            this.form.isTop = true
          } else {
            this.form.isTop = false
          }
          this.drawerLoading = false
        } else {
          Message.warning(res.header.message)
        }
      })
    },
    clean() {
      this.form = {
        informationName: '',
        endDate: '',
        informationType: '',
        id: 0,
        orderByField: '',
        pageNum: 0,
        pageSize: 0,
        releaseScope: [],
        rowStart: 0,
        startDate: '',
        uploadFileName: [],
        isTop: false,
        msgContent: '',
        appointTime: '',
      }
      this.fileList = []
      this.reviseId = ''
      this.dateForm.appointTime = ''
      if (this.$refs['form'] !== undefined) {
        this.$refs['form'].resetFields();
      }
      if (this.$refs['dateForm'] !== undefined) {
        this.$refs['dateForm'].resetFields();
      }
    },
    cancelNewData() {
      // this.$refs.wangeditor.clean();
      this.clean()
      this.$emit('isShowNewData', false)
    },
    handleCheckAllChange(val) {
      console.log(val)
      this.form.releaseScope = val ? this.allRoleList.map((item) => item.id) : []
      this.isIndeterminate = false
    },
    handleCheckedCitiesChange(value) {
      let { allRoleList } = this
      let { releaseScope } = this.form
      this.checkAll = releaseScope.length === allRoleList.length && allRoleList.length > 0
      this.isIndeterminate = releaseScope.length > 0 && releaseScope.length < allRoleList.length
    },
    // 预览
    preview() {
      this.isshowMessage = true
      this.$refs.content.announcementpreview(this.form, 'new')
    },
    //正文内容追加附件
    addAttachments(fileName,url) {
      let str1 = '<br><p> 附件：';
      let str2 = '';
      if(this.form.msgContent.indexOf('附件：') != -1){
        str1 = '<p>';
        str2 += '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;';
      }
      str2 += '<a href="'+url+'" target="_blank">'+fileName +'</a>';
      let str3 = '</p>';
      this.form.msgContent+=str1+str2+str3;
      // this.$refs.wangeditor.html = this.form.msgContent
    },
    // 预约发布
    subscribe() {
      this.$refs['form'].validate((valid) => {
        if (valid) {
          this.dialogVisible = true
        }
      })
    },
  },
}
</script>

<style lang="less" scoped>
/deep/.el-divider--horizontal {
  background-color: #1890ff;
}
/deep/.el-card__body {
  width: 100%;
  height: 100%;
}
.cardhei {
  height: 100%;
  .divbody {
    width: 100%;
    height: 100%;
    position: relative;
    .butPosition {
      display: flex;
      justify-content: flex-end;
    }
  }
}
.checkboxFlex {
  display: flex;
  flex-direction: column;
}
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
.dialogbody {
  display: flex;
  justify-content: center;
  padding-top: 15px;
}
</style>
