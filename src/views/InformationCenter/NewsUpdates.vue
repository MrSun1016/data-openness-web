<template>
  <div>
    <div v-show="!isNewData && !isshowMessage">
      <el-card class="box-card">
        <el-form :inline="true" ref="learning" size="small" :model="learning" class="farderfromflex">
          <div class="divffromflex">
            <el-form-item label="新闻标题：" prop="informationName">
              <el-input v-model="learning.informationName" placeholder="请输入" size="small"></el-input>
            </el-form-item>
            <el-form-item label="状态：" prop="releaseState">
              <el-select v-model="learning.releaseState" clearable placeholder="请选择" size="small">
                <el-option
                  :label="manag.text"
                  :value="manag.value"
                  v-for="(manag, index) in datefromList.management"
                  :key="manag.value"
                ></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="发布时间：">
              <el-date-picker
                v-model="datepicker"
                :default-time="['00:00:00', '23:59:59']"
                value-format="yyyy-MM-dd HH:mm:ss"
                type="daterange"
                range-separator="至"
                start-placeholder="开始日期"
                end-placeholder="结束日期"
                class="datawei"
              >
              </el-date-picker>
            </el-form-item>
            <el-form-item size="large">
              <el-button type="primary" size="mini" class="buthei" @click="onSubmit">查 询</el-button>
              <el-button size="mini" class="buthei" @click="resetForm('learning')">重 置</el-button>
            </el-form-item>
          </div>
        </el-form>
      </el-card>
      <el-card class="cardmargtop">
        <el-button type="primary" size="small" @click="addRelease" v-has="'isNewsUpdates:issueData'"
          >发布新闻</el-button
        >
        <el-button
          type="primary"
          size="small"
          @click="deleteDraft = true"
          :disabled="deleteAll"
          v-has="'isNewsUpdates:allDelete'"
          >批量删除</el-button
        >
        <el-table
          :header-cell-style="{
            background: '#E6F7FF',
            fontWeight: '600',
            color: '#333333',
            fontSize: '14px',
          }"
          :data="tabData"
          style="width: 100%"
          class="tablemargin"
          @selection-change="handleSelect"
          :row-class-name="tableRowClassName"
        >
          <el-table-column type="selection" align="center"> </el-table-column>
          <el-table-column label="新闻标题" :show-overflow-tooltip="true">
            <template slot-scope="{ row, $index }">
              <el-button type="text" slot="reference" class="itemSlotheden2" @click="preview(row)">{{ row.informationName }}</el-button>
              <!-- <i class="el-icon-top"></i> -->
            </template>
          </el-table-column>
          <el-table-column prop="releaseTime" label="发布时间"> </el-table-column>
          <el-table-column prop="releaseState" label="状态"> </el-table-column>
          <el-table-column prop="releasePerson" label="操作人"> </el-table-column>
          <el-table-column label="操作">
            <template slot-scope="{ row, $index }">
              <el-button type="text" slot="reference" @click="deleteWorld(row)" v-has="'isNewsUpdates:delete'"
                >删除</el-button
              >
              <el-button type="text" slot="reference" @click="modifyNewData(row)" v-has="'isNewsUpdates:change'"
                >修改</el-button
              >
            </template>
          </el-table-column>
        </el-table>

        <!-- 分页器 -->
        <el-pagination
          class="paginationtop"
          background
          @size-change="handleSizeChange"
          @current-change="getLearningWorld"
          :current-page="learning.pageNum"
          :page-sizes="[10, 20, 50, 100]"
          :page-size="learning.pageSize"
          :total="total"
          :pager-count="5"
          layout="->,total, prev, pager, next, sizes, jumper"
        >
        </el-pagination>
        <!-- 删除 -->
        <el-dialog title="提示" :visible.sync="deleteDraft" width="30%">
          <el-divider class="dividerhrcol"></el-divider>
          <p class="dialogcenter" v-show="informationId.length != 0">
            <i class="el-icon-info iconcolor"></i> 是否确认删除该资讯？
          </p>
          <p class="dialogcenter" v-show="informationId.length == 0">
            <i class="el-icon-info iconcolor"></i> 是否确认批量删除该资讯？
          </p>
          <span slot="footer" class="dialog-footer">
            <el-button size="small" @click="deleteDraft = false">取 消</el-button>
            <el-button size="small" type="primary" @click="deleteDraftli">确 认</el-button>
          </span>
        </el-dialog>
      </el-card>
    </div>
    <InformationNewData ref="dataFrom" :datefromList="datefromList" v-show="isNewData" @isShowNewData="isShowNewData" />
    <!-- <PreviewContent ref="content" v-show="isshowMessage" :visible.sync="isshowMessage" /> -->
    <MessageContent ref="content" v-show="isshowMessage" :visible.sync="isshowMessage"></MessageContent>
  </div>
</template>
<script>
import InformationNewData from '@/views/InformationCenter/InformationNewData'
import { informationPage, getInformationInfo } from '@/api/api'
import { deleteInformation } from '@/api/department'
import { mapState } from 'vuex'
import { MessageBox, Message } from 'element-ui'
// import PreviewContent from './PreviewContent.vue'
import MessageContent from './MessageContent'
export default {
  name: 'NewsUpdates',
  components: {
    InformationNewData,
    // PreviewContent,
    MessageContent,
  },
  data() {
    return {
      deleteAll: true,
      learning: {
        informationName: '',
        id: 0,
        orderByField: '',
        pageNum: 1,
        pageSize: 10,
        rowStart: 0,
        releaseScope: [],
        startDate: '',
        endDate: '',
        uploadFileName: '',
        releaseState: '',
        informationType: '1',
      },
      total: 0,
      datepicker: [],
      selectTableID: [],
      tabData: [],
      isNewData: false,
      deleteDraft: false,
      informationId: [],
      // 数据字典
      management: 'release_state',
      isshowMessage: false,
      contenItem: {
        titile: '',
        createdTime: '',
        deptName: '',
        viewNum: '',
        msgAbstract: '',
        msgContent: ''
      },
    }
  },
  mounted() {
    this.getFrom()
    this.getLearningWorld()
  },
  methods: {
    getFrom() {
      // 数源应用系统
      this.$store.dispatch('getManagement', this.management)
    },
    // 查 询
    onSubmit() {
      if (this.datepicker) {
        if (this.datepicker.length != 0) {
          this.learning.startDate = this.datepicker[0]
          this.learning.endDate = this.datepicker[1]
        }
      } else {
        this.learning.startDate = ''
        this.learning.endDate = ''
      }
      this.learning.pageNum = 1
      this.getLearningWorld()
    },
    resetForm(learning) {
      this.$refs[learning].resetFields()
      this.learning = {
        informationName: '',
        id: 0,
        orderByField: '',
        pageNum: 1,
        pageSize: 10,
        rowStart: 0,
        releaseScope: [],
        startDate: '',
        endDate: '',
        uploadFileName: '',
        releaseState: '',
        informationType: '1',
      }
      this.datepicker = []
    },
    handleSizeChange(pageSize) {
      this.learning.pageSize = pageSize
      this.getLearningWorld()
    },
    // 根据选择加当前页发请求
    getLearningWorld(page = 1) {
      this.learning.pageNum = page
      informationPage(this.learning).then((res) => {
        if (res.success) {
          this.tabData = res.body.content
          this.total = res.body.total
          this.tabData.map((item, index) => {
            item.releaseTime = this.timeCycle(item.releaseTime)
          })
        } else {
          this.$message.warning(res.message)
        }
      })
    },

    handleSelect(row) {
      this.selectTableID = []
      if (row.length > 0) {
        this.deleteAll = false
        row.forEach((value) => {
          this.selectTableID.push(value.id)
        })
      }else  this.deleteAll = true
    },
    // 点击修改
    modifyNewData(row) {
      this.isNewData = true
      this.$refs.dataFrom.reviseDate(row.id)
    },
    // 删除
    deleteWorld(row) {
      this.informationId.push(row.id)
      this.deleteDraft = true
    },
    deleteDraftli() {
      if (this.informationId.length != 0) {
        deleteInformation(this.informationId).then((res) => {
          if (res.success) {
            Message({
              message: '删除成功！',
              type: 'success',
            })
            this.deleteDraft = false
            this.getLearningWorld()
          } else {
            this.deleteDraft = false
            this.$message.warning(res.header.message)
          }
        })
      } else {
        deleteInformation(this.selectTableID).then((res) => {
          if (res.success) {
            Message({
              message: '删除成功！',
              type: 'success',
            })
            this.deleteDraft = false
            this.getLearningWorld()
          } else {
            this.deleteDraft = false
            this.$message.warning(res.header.message)
          }
        })
      }
      this.informationId = []
      this.selectTableID = []
    },
    isShowNewData(val) {
      this.isNewData = val
      this.getLearningWorld()
    },
    // 选中背景色
    tableRowClassName({ row }) {
      let color = ''
      for (let item of this.selectTableID.values()) {
        if (item === row.id) {
          color = 'table-SelectedRow-bgcolor'
        }
      }
      return color
    },
    //时间转换
    timeCycle(da) {
      if (da != null) {
        var date = new Date(da)
        var Y = date.getFullYear() + '-'
        var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-'
        var D = (date.getDate() < 10 ? '0' + date.getDate() : date.getDate()) + ' '
        var h = date.getHours() < 10 ? '0' + date.getHours() + ':' : date.getHours() + ':'
        var m = (date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()) + ':'
        var s = date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds()
        return Y + M + D + h + m + s
      }
    },
    // 预览
    preview(row) {
      this.isshowMessage = true
      // getInformationInfo(row.id).then((res) => {
      //   if (res.success) {
      //     this.contenItem.createdTime = this.timeCycle(res.result.createdTime)
      //     this.contenItem.deptName = res.result.deptName
      //     this.contenItem.viewNum = res.result.viewNum
      //     this.contenItem.msgContent = res.result.msgContent
      //   }
      // });
      // this.$refs.content.announcementpreview(this.contenItem, 'new')
      this.$refs.content.getContent(row)
    },
    // 新增发布信息
    addRelease() {
      this.$refs.dataFrom.clean();
      this.$refs.dataFrom.form.title = '新增新闻动态';
      this.$refs.dataFrom.form.informationType = '1';
      this.isNewData = true;
    },
  },
  computed: {
    ...mapState({
      datefromList: (state) => state.indatafrom.datefromList,
    }),
  },
}
</script>

<style lang="less" scoped>
.divffromflex {
  display: flex;
  justify-content: space-between;
}
/deep/.el-range-separator {
  margin: 0 15px 0 0;
}
/deep/.table-SelectedRow-bgcolor {
  td {
    background: rgba(24, 144, 255, 0.12) !important;
  }
}
.tablemargin {
  margin-top: 20px;
}
.cardmargtop {
  margin-top: 25px;
}
.datawei {
  width: 280px;
}

.el-divider--horizontal {
  margin: 0 0 5px 0;
}
/deep/.el-dialog__title {
  color: #1890ff;
}
/deep/.el-divider--horizontal {
  background-color: #1890ff;
}
/deep/.el-dialog__body {
  padding: 0;
}
/deep/.el-icon-info {
  width: 26px;
  height: 26px;
  color: #1890ff;
}
.dialogcenter {
  text-align: center;
  line-height: 200px;
  font-size: 16px;
  color: #000000;
}
.itemSlotheden2 {
  width: 100%;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  text-align:left;
}
</style>
