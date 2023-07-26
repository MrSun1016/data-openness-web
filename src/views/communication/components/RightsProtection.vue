<template>
  <div id="informationContent">
    <div v-show="!isshowDetails">
      <el-table
        :data="tableData"
        :row-style="{ height: '60px' }"
        :header-cell-style="{ background: '#DDE1E7', color: '#333333' }"
        style="width: 100%"
      >
        <el-table-column type="index" align="center" width="50" label="序号"></el-table-column>
        <el-table-column prop="name" align="center" width="110" label="保护方式"> </el-table-column>
        <el-table-column prop="address" align="center" label="数据资源">
          <template slot-scope="{ row }">
            <span :style="{ color: '#3685EA', cursor: 'pointer' }" @click="toDetails">
              {{ row.address }}
            </span>
          </template>
        </el-table-column>
        <el-table-column prop="datasource" align="center" label="数源部门"> </el-table-column>
        <el-table-column prop="modeDelivery" align="center" width="100" label="提供方式"> </el-table-column>
        <el-table-column prop="state" align="center" width="100" label="状态"> </el-table-column>
        <el-table-column prop="date" align="center" label="申请日期"> </el-table-column>
      </el-table>
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
    <applicationDetails class="details" v-show="isshowDetails" @closeDetails="closeDetails"></applicationDetails>
  </div>
</template>

<script>
import applicationDetails from '@/views/communication/components/applicationDetails'
export default {
  name: 'RightsProtection',
  components: {
    applicationDetails
  },
  data() {
    return {
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
      isshowDetails: false
    }
  },
  methods: {
    handleSizeChange() {},
    handleCurrentChange() {},
    // 点击名字
    toDetails() {
      this.isshowDetails = true
    },
    // 关闭详情
    closeDetails(val) {
      this.isshowDetails = val
    }
  }
}
</script>
<style></style>
<style lang="less" scoped>
#informationContent {
  width: 100%;
  padding: 0 24px;
}
/deep/.el-table tr {
  background-color: #f7f7fa !important;
}
.pagination {
  margin: 20px 0;
  text-align: center;
}
.details {
  width: 100%;
  // padding: 0 24px;
}
</style>
