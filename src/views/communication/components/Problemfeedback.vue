<template>
  <div id="Problemfeedback">
    <div v-show="!isshowFeedback">
      <el-tabs v-model="activeName">
        <el-tab-pane label="用户管理" name="first">
          <!-- :row-class-name="tableRowClassName" -->
          <el-table
            :data="tableData"
            @row-click="toProblem"
            :row-style="{ height: '60px' }"
            :header-cell-style="{ background: '#DDE1E7', color: '#333333' }"
            style="width: 100%"
          >
            <el-table-column type="index" align="center" width="50" label="序号"></el-table-column>
            <el-table-column prop="datasource" align="center" label="数源部门"> </el-table-column>
            <el-table-column prop="state" align="center" label="状态">
              <template slot-scope="{ row }">
                <span :style="{ color: '#3685EA', cursor: 'pointer' }">
                  {{ row.state }}
                </span>
              </template>
            </el-table-column>
            <el-table-column prop="date" align="center" label="申请日期"> </el-table-column>
          </el-table>
        </el-tab-pane>
        <el-tab-pane label="配置管理" name="second">
          <!-- :row-class-name="tableRowClassName" -->
          <el-table
            :data="tableData"
            @row-click="toProblem"
            :row-style="{ height: '60px' }"
            :header-cell-style="{ background: '#DDE1E7', color: '#333333' }"
            style="width: 100%"
          >
            <el-table-column type="index" align="center" width="50" label="序号"></el-table-column>
            <el-table-column prop="datasource" align="center" label="数源部门"> </el-table-column>
            <el-table-column prop="state" align="center" label="状态">
              <template slot-scope="{ row }">
                <span :style="{ color: '#3685EA', cursor: 'pointer' }">
                  {{ row.state }}
                </span>
              </template>
            </el-table-column>
            <el-table-column prop="date" align="center" label="申请日期"> </el-table-column>
          </el-table>
        </el-tab-pane>
      </el-tabs>
      <el-pagination
        class="pagination"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="pageNum"
        :page-sizes="[10, 20, 30, 50]"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        background
        :total="total"
      >
      </el-pagination>
    </div>
    <FeedbackDetails class="feedback" v-show="isshowFeedback" @closeFeedback="closeFeedback"></FeedbackDetails>
  </div>
</template>
<script>
import FeedbackDetails from '@/views/communication/components/FeedbackDetails'

export default {
  name: 'Problemfeedback',
  components: {
    FeedbackDetails
  },
  data() {
    return {
      activeName: 'first',
      tableData: [
        {
          name: '下架',
          address: '附件数据属性结构描述表',
          datasource: '孝感不动产登记中心',
          modeDelivery: 'API',
          state: '已回复',
          date: '2016-05-02'
        },
        {
          name: '下架',
          address: '附件数据属性结构描述表',
          datasource: '孝感不动产登记中心',
          modeDelivery: 'API',
          state: '已回复',
          date: '2016-05-02'
        },
        {
          name: '下架',
          address: '附件数据属性结构描述表',
          datasource: '孝感不动产登记中心',
          modeDelivery: 'API',
          state: '已回复',
          date: '2016-05-02'
        },
        {
          name: '下架',
          address: '附件数据属性结构描述表',
          datasource: '孝感不动产登记中心',
          modeDelivery: 'API',
          state: '已回复',
          date: '2016-05-02'
        }
      ],
      pageNum: 1,
      pageSize: 10,
      total: 100,
      isshowFeedback: false
    }
  },
  methods: {
    handleSizeChange() {},
    handleCurrentChange() {},
    toProblem(row) {
      this.isshowFeedback = true
      console.log('rowtoProblem', row)
    },
    closeFeedback(val) {
      this.isshowFeedback = val
    },
    tableRowClassName({ row, rowIndex }) {
      row.index = rowIndex
    }
  }
}
</script>

<style lang="scss" scoped>
#Problemfeedback {
  width: 100%;
  padding: 0 24px;
}
.pagination {
  margin: 20px 0;
  text-align: center;
}
/deep/.el-tabs__nav-wrap::after {
  background: transparent !important;
}
/deep/.el-tabs__item.is-active {
  background: #f5f5f9;
}
/deep/ .el-tabs__item {
  padding: 0px;
  width: 90px;
  text-align: center;
}
/deep/ .el-tabs__active-bar {
  width: 90px !important;
}
.feedback {
  width: 100%;
}
</style>
