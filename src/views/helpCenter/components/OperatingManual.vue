<template>
  <div>
    <el-table
      :data="tableData"
      @row-click="toProblem"
      :row-style="{ height: '60px' }"
      :header-cell-style="{ background: '#DDE1E7', color: '#333333' }"
      style="width: 100%"
    >
      <el-table-column type="index" align="center" width="50" label="序号"></el-table-column>
      <el-table-column prop="datasource" align="center" label="操作手册">
        <template slot-scope="{ row }">
          <span :style="{ color: '#3685EA', cursor: 'pointer' }">
            {{ row.datasource }}
          </span>
        </template>
      </el-table-column>
      <el-table-column prop="state" align="center" label="发布人"> </el-table-column>
      <el-table-column prop="date" align="center" label="发布时间"> </el-table-column>
    </el-table>
  </div>
</template>
<script>
export default {
  name: 'OperatingManual',
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
      ]
    }
  }
}
</script>

<style lang="less" scoped>
#OperatingManual {
  padding: 24px;
  width: 100%;
  margin: 0 20px;
  background-color: #fafafc;
}
</style>
