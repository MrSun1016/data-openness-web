<template>
  <div>
    <div v-show="!isshowNewAnnoun && !isshowContent">
      <el-card>
        <el-form :inline="true" :model="cement" size="small" ref="cement" class="demo-form-inline faderfrom">
          <div class="divffromflex">
            <el-form-item label="申请标题" prop="title" class="topfromitem">
              <el-input size="small" v-model="cement.applicationTitle" placeholder="请输入" class="aitemml"></el-input>
            </el-form-item>
            <el-form-item label="申请理由" prop="title" class="topfromitem">
              <el-input size="small" v-model="cement.applicationReason" placeholder="请输入" class="aitemml"></el-input>
            </el-form-item>

            <el-form-item label="提交时间：">
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
            <el-form-item class="fromweizhi">
              <el-button type="primary" size="small" @click="submitexcel" class="butfromccwei">查 询</el-button>
              <el-button size="small" @click="resetexcel('cement')" class="butfromccwei">重 置</el-button>
            </el-form-item>
          </div>
        </el-form>
      </el-card>
      <el-card class="cardmargtop">
        <el-button class="butPrimary" type="primary" size="small" @click="newAdd" v-has="'dataApplication:save'"
          >新增申请</el-button>
        <el-dropdown trigger="click" class="piliangfz">
            <el-button type="text" class="piliangbut">
              批量操作 <i class="el-icon-caret-bottom el-icon--right"></i>
            </el-button>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item class="buttblok" @click.native="isShowExcleLeadingin" v-has="'dataApplication:update'"
                >批量导入</el-dropdown-item
              >
              <el-dropdown-item class="buttblok" @click.native="isShowBatchExport" v-has="'dataApplication:delete'"
                >批量导出</el-dropdown-item
              >
            </el-dropdown-menu>
        </el-dropdown>

        <el-table
          :header-cell-style="{
            background: '#E6F7FF',
            fontWeight: '600',
            color: '#333333',
            fontSize: '14px',
          }"
          border
          stripe
          v-loading="loading"
          :data="tabData"
          style="width: 100%"
          @selection-change="handleSelect"
          :row-class-name="tableRowClassName"
        >
          <el-table-column type="selection" align="center" min-width="5%"> </el-table-column>
          <el-table-column prop="sn" label="序号" min-width="5%"></el-table-column>
          <el-table-column prop="applicationTitle" label="申请标题" min-width="15%"> 
                <template slot-scope="{ row }">
                    <el-button type="text" class="itemSlotheden2" @click="isonview(row)">{{ row.applicationTitle || '-' }}</el-button>
                </template>
          </el-table-column>
          <el-table-column prop="applicationReason" class="over-item" label="申请理由" min-width="20%"> </el-table-column>
          <el-table-column prop="realName" class="over-item" label="用户名称" min-width="10%"> </el-table-column>
          <el-table-column prop="dataFormat" class="over-item" label="提供方式" min-width="10%"> </el-table-column>
          <el-table-column prop="applicationStatus" label="状态" min-width="10%"> </el-table-column>
          <el-table-column prop="createdTime" label="申请时间" min-width="15%"> </el-table-column>
          <el-table-column label="操作" min-width="10%">
            <template slot-scope="{ row }">
              <div class="tempFlex">
                <div @click="isondelete(row)" class="tabnamewei margdiv" v-has="'dataApplication:delete'">删除</div>

                <div @click="isonupdate(row)" class="tabnamewei margdiv" v-has="'dataApplication:update'">修改</div>
              </div>
            </template>
          </el-table-column>
        </el-table>

        <!-- 分页器 -->
        <el-pagination
          class="paginationtop"
          background
          @size-change="handleSize"
          @current-change="getRolesTable"
          :current-page="cement.pageNum"
          :page-sizes="[10, 20, 50, 100]"
          :page-size="cement.pageSize"
          :total="total"
          :pager-count="5"
          layout="->,total, prev, pager, next, sizes, jumper"
        >
        </el-pagination>
      </el-card>
      <!-- 保存 -->
      <el-dialog title="保存提示" :visible.sync="isShowdelete" width="30%">
        <el-divider></el-divider>
        <p class="dialogcenter">是否确认删除?</p>
        <span slot="footer" class="dialog-footer">
          <el-button @click="cancelDelete" size="small" class="deletmarg">取 消</el-button>
          <el-button type="primary" size="small" @click="isDelete">确 认</el-button>
        </span>
      </el-dialog>
    </div>
    <!-- 新增 -->
    <NewApplication
      ref="NewAnnoun"
      v-show="isshowNewAnnoun"
      :visible.sync="isshowNewAnnoun"
      @refresh="refresh"
    ></NewApplication>
    <!-- 预览 -->
    <DataApplicationDetail ref="content" v-show="isshowContent"   :visible.sync="isshowContent"></DataApplicationDetail>
  </div>
</template>
<script>
import { MessageBox, Message } from 'element-ui'
import { mapState } from 'vuex'
import { applicationList,delApplication } from '@/api/api'
import NewApplication from './modules/NewApplication'
import DataApplicationDetail from './modules/DataApplicationDetail'
export default {
  name: 'DataApplication',
  components: {
    NewApplication,
    DataApplicationDetail,
  },
  data() {
    return {
      cement: {
        sn: '',
        applicationTitle: '',
        applicationReason: '',
        realName: '',
        dataFormat: '',
        status: '',
        applicationStatus: '',
        pageNum: 1,
        pageSize: 10,
        createdTime: '',
        startTime: '',
        endTime: '',
      },
      total: 0,
      datepicker: [],
      tabData: [],
      loading: false,
      selectTableID: [],
      // 预览
      isshowContent: false,
      // 新增
      isshowNewAnnoun: false,
      // 删除
      isShowdelete: false,
      ids: '',
    }
  },
  created() {
    this.getRolesTable()
  },
  methods: {
    //详情
    isonview(row){
        this.isshowContent = true
        this.$refs.content.getContent(row)
    },
    //编辑
    isonupdate(row){
        this.isshowNewAnnoun = true
        this.$refs.NewAnnoun.duplicateAnnounce(row)
    },
    // 删除
    isondelete(row) {
      this.isShowdelete = true
      this.ids = row.id
    },
    // 确认删除
    isDelete() {
      let delId
      if (this.ids != '') {
        delId = this.ids
      } else {
        delId = this.selectTableID
      }
      delApplication(delId).then((res) => {
        if (res.success) {
          Message({
            message: "删除成功!",
            type: 'success',
          })
          this.isShowdelete = false
          this.getRolesTable()
        } else {
          this.isShowdelete = false
          Message.error("删除失败!")
        }
      })
      this.ids = ''
    },
    cancelDelete() {
      this.isShowdelete = false
      this.ids = ''
    },
    // 新增
    newAdd() {
      this.isshowNewAnnoun = true
    },
    // 复制
    duplicate(row) {
      this.isshowNewAnnoun = true
      this.$refs.NewAnnoun.duplicateAnnounce(row)
    },
    refresh() {
      this.getRolesTable()
      this.loading = true
    },
    // 分页
    getRolesTable(page = 1) {
      this.loading = true
      this.cement.pageNum = page
      if (this.datepicker) {
        if (this.datepicker.length != 0) {
          this.cement.startTime = this.datepicker[0]
          this.cement.endTime = this.datepicker[1]
        }
      } else {
        this.cement.startTime = ''
        this.cement.endTime = ''
      }
      applicationList(this.cement)
        .then((res) => {
          if (res.success) {
            this.tabData = res.body.content
            this.total = res.body.total
            this.tabData.forEach((item, index) => {
              item.createdTime = this.formatTime(item.createdTime)
            })
            this.loading = false
          } else this.loading = false
        })
        .catch((_) => {
          this.loading = false
        })
    },
    handleSize(pageSize) {
      this.cement.pageSize = pageSize
      this.getRolesTable()
    },
    submitexcel() {
      this.getRolesTable()
    },
    resetexcel(excelData) {
      this.$refs[excelData].resetFields()
      this.datepicker = []
    },
    handleSelect(row) {
      this.selectTableID = []
      if (row.length > 0) {
        row.forEach((value) => {
          this.selectTableID.push(value.id)
        })
      }
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
    getCurrentTime() {
      let year = new Date().getFullYear()
      let month = new Date().getMonth() + 1 < 10 ? '0' + (new Date().getMonth() + 1) : new Date().getMonth() + 1
      let day = new Date().getDate() < 10 ? '0' + new Date().getDate() : new Date().getDate()
      let hours = new Date().getHours() < 10 ? '0' + new Date().getHours() : new Date().getHours()
      let minutes = new Date().getMinutes() < 10 ? '0' + new Date().getHours() : new Date().getHours()
      return year + '-' + month + '-' + day + ' ' + hours + ':' + minutes
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
.itemSlotheden2 {
  width: 115px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  text-align:left;
}
.divffromflex {
  display: flex;
  justify-content: space-between;
}

/deep/.el-range-separator {
  margin: 0 15px 0 0;
}
.datawei {
  width: 280px;
}
.cardmargtop {
  margin-top: 25px;
  .butPrimary {
    margin: 0 20px 20px 0;
  }
}
.tabnamewei {
  margin: 0;
  color: #409eff;
  cursor: pointer;
}
.tabname {
  color: #ff0000;
  cursor: pointer;
}
.margdiv {
  margin-right: 10px !important;
}
.tempFlex {
  display: flex;
  .topdiv {
    width: 60px;
    margin-right: 10px !important;
  }
}
/deep/.table-SelectedRow-bgcolor {
  td {
    background: rgba(24, 144, 255, 0.12) !important;
  }
}
.paginationtop {
  margin-top: 10px;
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
.dialogcenter {
  text-align: center;
  line-height: 200px;
  font-size: 16px;
  color: #000;
}
.piliangfz {
  position: relative;
  width: 120px;
  // margin-right: 20px;
}
.piliangbut {
  position: relative;
  width: 120px;
  height: 32px;
  border-radius: 4px;
  border: 1px solid #1890ff;
  color: #1890ff;
  // margin-right: 20px;
  line-height: 32px;
  font-size: 14px;
  padding: 0 17px;
  i {
    margin-left: 15px;
    color: #1890ff;
  }
}
/deep/.el-dropdown-menu__item {
  color: #1890ff;
}
/deep/.el-dropdown-menu__item.is-disabled {
  color: #bbb;
}
.itme-title {
  width: 160px;
  display: inline-block;
}
.over-item {
  cursor: pointer;
  width: 20%;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>
