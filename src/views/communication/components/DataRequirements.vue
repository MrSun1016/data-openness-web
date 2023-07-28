<template>
  <div id="DataRequirements">
    <div v-show="!isshowRequirement">
      <el-table
        :data="tableData"
        @row-click="toResponse"
        :row-style="{ height: '60px' }"
        :header-cell-style="{ background: '#DDE1E7', color: '#333333' }"
        style="width: 100%"
      >
        <el-table-column type="index" align="center" width="50" label="序号"></el-table-column>
        <!-- <el-table-column prop="name" align="center" width="110" label="申请标题"> </el-table-column> -->
        <el-table-column prop="address" align="center" label="数据资源">
          <template slot-scope="{ row }">
            <span :style="{ color: '#3685EA', cursor: 'pointer' }">
              {{ row.address }}
            </span>
          </template>
        </el-table-column>
        <el-table-column prop="datasource" align="center" label="申请理由"> </el-table-column>
        <el-table-column prop="modeDelivery" align="center" width="100" label="提供方式"> </el-table-column>
        <el-table-column prop="state" align="center" width="100" label="状态">
          <template slot-scope="{ row }">
            <span :style="{ color: '#3685EA', cursor: 'pointer' }">
              {{ row.state }}
            </span>
          </template>
        </el-table-column>
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
    <RequirementResponse
      class="Response"
      v-show="isshowRequirement"
      @closeRequirement="closeRequirement"
    ></RequirementResponse>
  </div>
</template>
<script>
import RequirementResponse from '@/views/communication/components/RequirementResponse'
export default {
  name: 'DataRequirements',
  components: {
    RequirementResponse,
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
          date: '2016-05-02',
        },
        {
          name: '下架',
          address: '附件数据属性结构描述表',
          datasource: '孝感不动产登记中心',
          modeDelivery: 'API',
          state: '已回复',
          date: '2016-05-02',
        },
        {
          name: '下架',
          address: '附件数据属性结构描述表',
          datasource: '孝感不动产登记中心',
          modeDelivery: 'API',
          state: '已回复',
          date: '2016-05-02',
        },
        {
          name: '下架',
          address: '附件数据属性结构描述表',
          datasource: '孝感不动产登记中心',
          modeDelivery: 'API',
          state: '已回复',
          date: '2016-05-02',
        },
      ],
      pageNum: 1,
      pageSize: 10,
      total: 100,
      isshowRequirement: false,
    }
  },
  methods: {
    handleSizeChange() {},
    handleCurrentChange() {},
    toResponse(row) {
      this.isshowRequirement = true
    },
    closeRequirement(val) {
      this.isshowRequirement = val
    },
  },
}
</script>

<style lang="less" scoped>
#DataRequirements {
  width: 100%;
  padding: 0 24px;
}
.pagination {
  margin: 20px 0;
  text-align: center;
}
.Response {
  width: 100%;
}
</style>
