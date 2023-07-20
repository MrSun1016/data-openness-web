<template>
  <div id="Commentscoring">
    <div class="scoring">
      <span>评论打分</span>
      <el-rate v-model="value1"></el-rate>
    </div>
    <el-input type="textarea" :rows="5" v-model="commentContent" placeholder="请输入评论内容"> </el-input>
    <div class="postComments"><el-button type="primary">评论</el-button></div>
    <div class="commentDisplay">
      <div class="titlleinformation">
        <div class="titlleDisplay">
          <div>名字:</div>
          <el-rate v-model="scorerate" disabled :show-score="showscore"> </el-rate>
        </div>
        <div class="time">时间</div>
      </div>
      <div class="text">数据数据质量很好，对我帮助很大</div>
    </div>
    <!-- @size-change="handleSizeChange"
      @current-change="handleCurrentChange" -->
    <el-pagination
      :current-page="resource.pageNum"
      :page-sizes="[10, 20, 30, 50]"
      :page-size="resource.pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      background
      :total="total"
    >
    </el-pagination>
  </div>
</template>
<script>
export default {
  name: 'Commentscoring',
  data() {
    return {
      commentContent: '',
      scorerate: 2,
      showscore: false,
      resource: {
        pageNum: 1,
        pageSize: 10
      },
      total: 100
    }
  }
}
</script>

<style lang="less" scoped>
#Commentscoring {
  width: 100%;
  .scoring {
    margin-bottom: 15px;
    display: flex;
    align-items: center;
    span {
      margin-right: 15px;
    }
  }
  .postComments {
    margin: 20px 0;
    display: flex;
    flex-direction: row-reverse;
    button {
      width: 100px;
      background-color: #4888ff;
    }
  }
  .commentDisplay {
    width: 100%;
    // height: 60px;
    border-top: 1px solid #f0f3f7;
    .titlleinformation {
      margin: 15px 0;
      display: flex;
      justify-content: space-between;
      .titlleDisplay {
        display: flex;
        flex-direction: row;
        align-items: center;
        div {
          margin-right: 15px;
        }
      }
    }
    .text {
      margin-bottom: 15px;
    }
  }
  .el-pagination {
    text-align: center;
  }
}
</style>
