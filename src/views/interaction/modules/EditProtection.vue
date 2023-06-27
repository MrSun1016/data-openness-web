<template>
  <div>
    <el-card class="cardpaddin" v-show="!isshowMessage" v-loading="drawerLoading">
        <div class="titleflex">
          <p class="titlep">
            标签注： 当用户认为开放数据侵犯其商业秘密、个人隐私等合法权益的， 可以通过此页面功能告知数据开放主体， 并提交相关证据材料。
            开放主体会根据核实结果， 分别采取撤回数据，恢复开放或者处理后再开放等措施， 最终反馈给用户。</p>
        </div>
        <el-divider></el-divider>
      <div class="bodydiv">
        <el-form ref="form" :model="form" :rules="rules" label-position="left" label-width="80px">
          <el-form-item label="保护方式" key="4" class="fromtopmag" prop="protectionType">
              <el-select v-model="form.protectionType" size="small" placeholder="请选择">
                <el-option label="下架" value="下架"></el-option>
                <el-option label="脱敏" value="脱敏"></el-option>
              </el-select>
          </el-form-item>
          <el-form-item label="目录" prop="catalogName">
            <el-input size="small" v-model="form.catalogName"   @click.native="queryBAWO"></el-input>
          </el-form-item>
          <el-form-item label="内容" prop="protectionContent">
            <el-input
              type="textarea"
              :rows="3"
              maxlength="300"
              height="240"
              v-model="form.protectionContent"
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
      <!--数据资源列表
      <el-dialog :titleBAW="数据资源列表" :visible.sync="openBAW" width="500px" append-to-body
        :close-on-click-modal="false" :close-on-press-escape="false">
        <div>
          <el-table :data="fromB">
            <el-table-column label="数据目录" align="center" prop="catalogName" />
            <el-table-column label="数源单位" align="center" prop="sourceUnit" />
            <el-table-column label="数据格式" align="center" prop="dataFormat" />
          </el-table>
        </div>
      </el-dialog>-->


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
    <ProtectionDetail ref="content" v-show="isshowMessage" :visible.sync="isshowMessage"></ProtectionDetail>
  </div>
</template>
<script>
import { MessageBox, Message } from 'element-ui'
import { postProtectionAdd, getProtectionById, postProtectionEdit,queryBAWO } from '@/api/api'
import ProtectionDetail from './ProtectionDetail'
import { indexOf } from 'lodash'
export default {
  name: 'EditProtection',
  components: { ProtectionDetail },
  data() {
    return {
      catalogId: '',
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
        protectionType: '',
        protectionContent: '',
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
        protectionType: [{ required: true, message: '请选择保护方式', trigger: 'blur' }],
        catalogName: [{ required: true, message: '请输入信息资源名称', trigger: 'blur' }],
        protectionContent: [{ required: true, message: '请输入公告内容', trigger: 'blur' }],
      },
      loadSave: false,
      loadhandle: false,
      // 加载
      drawerLoading: false,
    }
  },
  mounted() {
    this.catalogId = this.$route.query.id
  },
  methods: {
    queryBAWO() {
      this.openBAW = true
     queryBAWO().then(response => {
        let res = [];
        res.push(response.data);
        this.fromB = res;
      });
    },
    handleChange(file, fileList) {
      let resFileName = file.name.split('.')
      this.fileName.fileName = resFileName[0]
    },
    // 编辑
    duplicateAnnounce(row) {
      this.drawerLoading = true
      this.duplicateId = row.id
      getProtectionById(row.id).then((res) => {
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
          }else{
            this.drawerLoading = false
          }
        } else {
            Message.error(res.header.message)
            this.drawerLoading = false
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
          postProtectionAdd(this.form).then((res) => {
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
          postProtectionEdit(this.form).then((res) => {
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
      console.log(this.form.attachmentDownload)
      if(this.form.attachmentDownload == '' || this.form.attachmentDownload == null){
          this.form.attachmentDownload = file.name
          this.form.attachmentDownload = file.name.split(" ")
      }else{
          if(this.form.attachmentDownload instanceof Array){
              this.form.attachmentDownload.push(file.name)
          }else{
              this.form.attachmentDownload = (this.form.attachmentDownload || "").split(" ")
              this.form.attachmentDownload.push(file.name)
          }

      }
      console.log(this.form.attachmentDownload)
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
