<template>
  <div id="myapplication">
    <div v-show="!isDetails">
      <div style="background-color: #edf1f6; padding: 15px">
        <!-- search -->
        <div class="search-box">
          <el-form :inline="true" :model="formInline" class="demo-form-inline">
            <el-form-item label="应用名称">
              <el-input v-model="formInline.user" placeholder="审批人" size="small"></el-input>
            </el-form-item>
            <el-form-item label="应用类型">
              <el-select v-model="formInline.region" placeholder="活动区域" size="small">
                <el-option label="类型一" value="shanghai"></el-option>
                <el-option label="类型二" value="beijing"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="应用状态">
              <el-select v-model="formInline.region" placeholder="活动区域" size="small">
                <el-option label="状态一" value="shanghai"></el-option>
                <el-option label="状态二" value="beijing"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="onSubmit" size="small">查询</el-button>
              <el-button type="primary" @click="onSubmit" size="small">重置</el-button>
            </el-form-item>
          </el-form>
        </div>
        <!-- table -->
        <el-button type="primary" @click="handleAdd" size="small" style="margin-bottom: 10px">新增</el-button>
        <el-table
          :data="tableData"
          style="width: 100%"
          :cell-style="handleCellStyle"
          :header-cell-style="{ background: '#DDE1E7' }"
        >
          <el-table-column type="selection" width="50"> </el-table-column>
          <el-table-column prop="name" label="应用名称" :show-overflow-tooltip="true">
            <template slot-scope="scope">
              <el-button type="text" size="small" @click="handleCutDet">{{ scope.row.name }}</el-button>
            </template>
          </el-table-column>
          <el-table-column prop="name" label="应用类型" :show-overflow-tooltip="true"> </el-table-column>
          <el-table-column prop="name" label="应用状态" :show-overflow-tooltip="true"> </el-table-column>
          <el-table-column prop="name" label="操作">
            <template slot-scope="scope">
              <el-button type="text" size="small">编辑</el-button>
              <el-button type="text" size="small">操作日志</el-button>
              <el-button type="text" size="small">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
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
    <ServiceDetails v-show="isDetails" @isServiceDetails="isServiceDetails" />
  </div>
</template>

<script>
import ServiceDetails from '@views/service/components/ServiceDetails'
export default {
  name: 'myapplication',
  components: {
    ServiceDetails,
  },
  data() {
    return {
      isDetails: false,
      formInline: {
        user: '',
        region: '',
      },
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
    handleCutDet(){
      this.isDetails = true
    },
    isServiceDetails(val) {
      this.isDetails = val
    },
    handleAdd() {
      this.$router.push('/personalCenter/personalAdd')
    },
    onSubmit() {
      console.log('submit!')
    },
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
#myapplication {
  width: 100%;
  margin: 0 24px;
  //   padding: 15px;
  //   background-color: #edf1f6;
  .pagination-box {
    margin: 10px 0;
    text-align: center;
  }
}
</style>