<template>
  <div>
    <el-card class="cardhei">
      <div>问题详情</div>
      <el-divider></el-divider>
      <div class="divbody">
        <el-form
          ref="form"
          :model="form"
          label-position="left"
          size="small"
          label-width="120px"
        >
          <p>
            <span class="itme-title">问题标题： </span><span>{{ consultItem.applicationTitle || '-' }}</span>
          </p>
          <p>
            <span class="itme-title">提交时间：</span><span>{{ consultItem.createdTime || '-' }}</span>
          </p>
          <p>
            <span class="itme-title">申请理由： </span>
            <span class="itme-cont">{{ consultItem.applicationReason || '-' }}</span>
          </p>
        </el-form>
        <div class="butPosition">
          <el-button size="small" @click="cancelNewData">取消</el-button>
        </div>
      </div>
    </el-card>
  </div>
</template>

<script>
import { getApplicationById } from '@/api/api'
import { mapState } from 'vuex'
import { MessageBox, Message } from 'element-ui'
export default {
  name: 'DataApplicationDetail',
  data() {
    return {
      consultItem: {
        applicationTitle: '',
        applicationReason: '',
        dataFormat: '',
        createdTime: ''
      },
    }
  },
  mounted() {},
  methods: {
    getContent(row) {
      getApplicationById(row.id).then(res => {
        if (res.success) {
          this.consultItem = res.body
          this.consultItem.createdTime = this.formatTime(this.consultItem.createdTime)
        } else {
          Message.error(res.message)
        }
      })
    },
    closeMessage() {
      this.consultItem = {}
      this.$emit('update:visible', false)
    },
    cancelNewData(){
       this.$emit('update:visible', false)
    }
  }
}
</script>

<style lang="less" scoped>
.itme-cont{
  width: 100%;
  height: 50%;
}
/deep/.el-divider--horizontal {
  background-color: #1890ff;
}
/deep/.el-card__body {
  width: 100%;
  height: 100%;
}
.cardhei {
  height: 100%;
  .divbody {
    width: 100%;
    height: 100%;
    position: relative;
    .butPosition {
      display: flex;
      justify-content: flex-end;
    }
  }
}
.checkboxFlex {
  display: flex;
  flex-direction: column;
}
.frombigweiht {
  width: 65vw;
}
.itme-title {
  padding: 32px;
  width: 160px;
  height: 45px;
  display: inline-block;
}
</style>
