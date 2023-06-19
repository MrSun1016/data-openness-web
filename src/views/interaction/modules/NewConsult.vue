<template>
  <div>
    <el-card class="cardpaddin" v-show="!isshowMessage" v-loading="drawerLoading">
      <div class="titleflex">
        <p class="titlep">新增问题反馈</p>
      </div>
      <el-divider></el-divider>
      <div class="bodydiv">
        <el-form ref="form" :model="form" :rules="rules" label-position="left" label-width="80px">
          <el-form-item label="反馈标题" prop="consultTitle">
            <el-input size="small" v-model="form.consultTitle"></el-input>
          </el-form-item>
          <el-form-item label="反馈类型" key="4" class="fromtopmag" prop="consultType">
            <el-select v-model="form.consultType" @change="changeAlign" placeholder="请选择" class="fromweiht">
              <el-option
                :label="openitem.text"
                :value="openitem.value"
                v-for="(openitem, index) in datefromList.consultType"
                :key="openitem.value"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="反馈内容" prop="consultContent">
            <el-input
              type="textarea"
              :rows="3"
              maxlength="300"
              height="240"
              v-model="form.consultContent"
              placeholder="请输入（20字以上）"
              class="frombigweiht"
            ></el-input>
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
    <ConsultDetail ref="content" v-show="isshowMessage" :visible.sync="isshowMessage"></ConsultDetail>
  </div>
</template>
<script>
import { MessageBox, Message } from 'element-ui'
import { postConsultAdd, getConsultById, postConsultEdit } from '@/api/api'
import ConsultDetail from './ConsultDetail'
import { mapState } from 'vuex'

export default {
  name: 'NewConsult',
  components: { ConsultDetail },
  data() {
    return {
      //   表单
      form: {
        id: '',
        consultTitle: '',
        consultContent: '',
        consultType: '',
      },
      dataDictionary: {
        // 反馈类型
        consultType: 'consult_type',
      },
      checkAll: false,
      isIndeterminate: false,
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
      rules: {
        consultTitle: [{ required: true, message: '请输入反馈标题', trigger: 'blur' }],
        consultType: [{ required: true, message: '请输入反馈类型', trigger: 'blur' }],
        consultContent: [{ required: true, message: '请输入反馈内容', trigger: 'blur' }],
      },
      loadhandle: false,
      // 加载
      drawerLoading: false,
    }
  },
  mounted() {
    // 新增页面选择框的数据-请求数据字典得到
    this.dictionaryList()
  },
  methods: {
    // 反馈类型
    changeAlign(val) {
      if (val != '') {
        this.form.consultType = val
      }
    },
     // 新增页面选择框的数据-请求数据字典得到
    dictionaryList() {
      let {
        consultType,
      } = this.dataDictionary

      // 反馈类型
      this.$store.dispatch('getConsultTypeList', consultType)
    },
    //编辑
    duplicateAnnounce(row) {
      this.drawerLoading = true
      getConsultById(row.id).then((res) => {
        if (res.success) {
          this.form = res.body
          this.drawerLoading = false
        }
      })
    },
    // 保存
    preserve() {
      if (this.form.consultTitle == '') {
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
          postConsultAdd(this.form).then((res) => {
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
          postConsultEdit(this.form).then((res) => {
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
        })
        }

      }
    },
    // 取消保存
    cancelSave() {
      this.isShowSave = false
    },
    cleanUp() {
      this.form = {
        consultTitle: '',
        consultContent: '',
        consultType: '',
      }
    },
    // 取消
    closeNewsAdd() {
      this.selectId = '0'
      this.isShowSave = true
    }
  },
  computed: {
    ...mapState({
      datefromList: state => state.indatafrom.datefromList
    })
  }
}
</script>

<style lang="less" scoped>
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
