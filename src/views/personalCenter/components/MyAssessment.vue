<template>
  <div id="myassessment">
    <el-table
      :data="tableData"
      style="width: 100%"
      :cell-style="handleCellStyle"
      :header-cell-style="{ background: '#DDE1E7' }"
    >
      <el-table-column type="index" label="序号" width="50"> </el-table-column>
      <el-table-column prop="name" label="数据资源" width="180" :show-overflow-tooltip="true"> </el-table-column>
      <el-table-column prop="name" label="数源部门" :show-overflow-tooltip="true"> </el-table-column>
      <el-table-column prop="name" label="数据格式" :show-overflow-tooltip="true"> </el-table-column>
      <el-table-column prop="ass" label="评价内容" :show-overflow-tooltip="true"> </el-table-column>
      <el-table-column prop="name" label="评价时间" :show-overflow-tooltip="true"> </el-table-column>
      <el-table-column prop="name" label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div class="pagination-box">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="learning.pageNum"
        :page-sizes="[10, 20, 30, 50]"
        :page-size="learning.pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        background
        :total="total"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
export default {
  name: 'myassessment',
  data() {
    return {
      tableData: [
        {
          date: '2016-05-02',
          name: '王小虎',
          ass: '啊啊啥',
          address: '上海市普陀区金沙江路 1518 弄',
        },
        {
          date: '2016-05-04',
          name: '王小虎',
          ass: '啊啊啥快乐就打了圣诞节啊啦卡德加拉德季拉胯捡垃圾大理石肯德基阿是啦快进到啊杀戮空间大啦看手机大垃圾打开老实交代啊流口水的',
          address: '上海市普陀区金沙江路 1517 弄',
        },
        {
          date: '2016-05-01',
          name: '王小虎',
          ass: '啊啊啥快乐就打了圣诞节啊啦卡德加拉德季拉胯捡垃圾大理石肯德基阿是啦快进到啊杀戮空间大啦看手机大垃圾打开老实交代啊流口水的',
          address: '上海市普陀区金沙江路 1519 弄',
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          ass: '啊啊啥快乐就打了圣诞节啊啦卡德加拉德季拉胯捡垃圾大理石肯德基阿是啦快进到啊杀戮空间大啦看手机大垃圾打开老实交代啊流口水的',
          address: '上海市普陀区金沙江路 1516 弄',
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          ass: '啊啊啥快乐就打了圣诞节啊啦卡德加拉德季拉胯捡垃圾大理石肯德基阿是啦快进到啊杀戮空间大啦看手机大垃圾打开老实交代啊流口水的',
          address: '上海市普陀区金沙江路 1516 弄',
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          ass: '啊啊啥快乐就打了圣诞节啊啦卡德加拉德季拉胯捡垃圾大理石肯德基阿是啦快进到啊杀戮空间大啦看手机大垃圾打开老实交代啊流口水的',
          address: '上海市普陀区金沙江路 1516 弄',
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          ass: '啊啊啥快乐就打了圣诞节啊啦卡德加拉德季拉胯捡垃圾大理石肯德基阿是啦快进到啊杀戮空间大啦看手机大垃圾打开老实交代啊流口水的',
          address: '上海市普陀区金沙江路 1516 弄',
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          ass: '啊啊啥快乐就打了圣诞节啊啦卡德加拉德季拉胯捡垃圾大理石肯德基阿是啦快进到啊杀戮空间大啦看手机大垃圾打开老实交代啊流口水的',
          address: '上海市普陀区金沙江路 1516 弄',
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          ass: '啊啊啥快乐就打了圣诞节啊啦卡德加拉德季拉胯捡垃圾大理石肯德基阿是啦快进到啊杀戮空间大啦看手机大垃圾打开老实交代啊流口水的',
          address: '上海市普陀区金沙江路 1516 弄',
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          ass: '啊啊啥快乐就打了圣诞节啊啦卡德加拉德季拉胯捡垃圾大理石肯德基阿是啦快进到啊杀戮空间大啦看手机大垃圾打开老实交代啊流口水的',
          address: '上海市普陀区金沙江路 1516 弄',
        },
      ],
      learning: {
        pageNum: 1,
        pageSize: 10,
      },
      total: 0,
    }
  },
  methods: {
    handleCellStyle({ row, column, rowIndex, columnIndex }) {
      return {
        'background-color': '#EDF1F6',
      }
    },
    handleSearch() {},
    handleSizeChange() {},
    handleCurrentChange() {},
  },
}
</script>

<style lang="less" scoped>
#myassessment {
  width: 100%;
  margin: 0 24px;
  //   background-color: #edf1f6;
  .pagination-box {
    margin: 10px 0;
    text-align: center;
  }
}
</style>