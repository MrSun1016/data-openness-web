<template>
  <div>
    <!--<el-card>
      <el-form :inline="true" :model="statistics" size="small" ref="statistics">
        <div class="divffromflex">
          <el-form-item label="表名称" prop="tableName">
            <el-input size="small" v-model="statistics.tableName" placeholder="请输入" class="aitemml"></el-input>
          </el-form-item>
        <el-form-item label="更新时间是否为空" prop="timeIsNull" class="fromweizhi">
          <el-select v-model="statistics.timeIsNull" size="small" clearable placeholder="请选择" class="aitemml">
            <el-option label="是" value="1"></el-option>
            <el-option label="否" value="2"></el-option>
          </el-select>
        </el-form-item>
          <el-form-item label="是否超时" prop="overTime">
            <el-select v-model="statistics.overTime" size="small" clearable placeholder="请选择" class="aitemml">
              <el-option label="是" value="1"></el-option>
              <el-option label="否" value="2"></el-option>
            </el-select>
          </el-form-item>
        </div>
        <div class="fromflexrig">
          <el-form-item>
            <el-button type="primary" size="small" @click="submitQuestion" class="butfromccwei">查 询</el-button>
            <el-button size="small" @click="resetexcel('statistics')" class="butfromccwei">重 置</el-button>
          </el-form-item>
        </div>
      </el-form>
    </el-card>-->
    <el-card class="cardpadmargtop">
      <el-table
        :header-cell-style="{
          background: '#FAFAFA',
          fontWeight: '400',
          color: '#333333',
          fontSize: '14px'
        }"
        border
        stripe
        v-loading="loading"
        :data="tabData"
        style="width: 100%"
      >
        <el-table-column prop="sn" label="序号"> </el-table-column>
        <el-table-column prop="demandTitle" label="需求标题"> </el-table-column>
        <el-table-column prop="dataUsage" label="数据用途"> </el-table-column>
        <el-table-column prop="dataFormat" label="数据格式"> </el-table-column>
        <el-table-column prop="createdTime" label="提交时间" :formatter="formatDate"> </el-table-column>
        <el-table-column label="操作"> 
          <template slot-scope="{ row, $index }">
              <div>
                <el-button
                  class="tabbutsize"
                  type="text"
                  @click="showDemand(row)"
                  >查看详情</el-button
                >
              </div>
            </template>
        </el-table-column>
      </el-table>
      <el-pagination
        class="paginationtop"
        background
        @size-change="handleSizeChange"
        @current-change="getRolesDraft"
        :current-page="statistics.pageNum"
        :page-sizes="[10, 20, 50, 100]"
        :page-size="statistics.pageSize"
        :total="total"
        :pager-count="5"
        layout="->,total, prev, pager, next, sizes, jumper"
      >
      </el-pagination>
    </el-card>
  </div>
</template>
<script>
import { demandList } from '@/api/api'
export default {
  name: 'UpdateTimeStatistics',
  data() {
    return {
      tabData: [],
      statistics: {
        pageNum: 1,
        pageSize: 10,
        timeIsNull: '',
        tableName: '',
        overTime: ''
      },
      total: 0,
      loading: false
    }
  },
  mounted() {
    this.getDemandList()
  },
  methods: {
    getDemandList(page = 1) {
      this.loading = true
      this.statistics.pageNum = page
      demandList(this.statistics).then(res => {
        if (res.success) {
          this.tabData = res.body.content
          this.total = res.body.total
          this.tabData.map((item, index) => {
            item.lastUpdateTime = this.formatTime(item.lastUpdateTime)
            item.oldTime = this.formatTime(item.oldTime)
          })
          this.loading = false
        }
      })
    },
    handleSizeChange(pageSize) {
      this.statistics.pageSize = pageSize
      this.getRolesDraft()
    },
    submitQuestion() {
      this.getRolesDraft()
    },
    formatDate:function(row, column){
      let data = row[column.property]
      if(data == null) {
        return null
      }
      let date = new Date(data);
      var o = { 
          "M+" : date.getMonth()+1,                 //月份 
          "d+" : date.getDate(),                    //日 
          "h+" : date.getHours(),                   //小时 
          "m+" : date.getMinutes(),                 //分 
          "s+" : date.getSeconds(),                 //秒 
          "q+" : Math.floor((date.getMonth()+3)/3), //季度 
          "S"  : date.getMilliseconds()             //毫秒 
      }; 
      var fmt = "yyyy-MM-dd hh:mm:ss";
      if(/(y+)/.test(fmt)) {
                fmt=fmt.replace(RegExp.$1, (date.getFullYear()+"").substr(4 - RegExp.$1.length)); 
        }
      for(var k in o) {
          if(new RegExp("("+ k +")").test(fmt)){
              fmt = fmt.replace(RegExp.$1, (RegExp.$1.length==1) ? (o[k]) : (("00"+ o[k]).substr((""+ o[k]).length)));
          }
      }
        return fmt ;
      }
  }
}
</script>

<style lang="less" scoped>
.cardpadmargtop {
  margin-top: 24px;
  padding-left: 20px;
  width: 100%;
}
.divffromflex {
  display: flex;
  justify-content: space-between;
}
.fromflexrig {
  display: flex;
  justify-content: flex-end;
  .el-form-item {
    margin-bottom: 5px;
  }
}
</style>
