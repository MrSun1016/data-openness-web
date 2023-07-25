<template>
  <div id="perCenter">
    <OpenPlatformHeader />
    <div class="content">
      <menus @handleSelect="handleSelect" />
      <!-- 基本信息 -->
      <basicMsg v-if="currentContent === 0" />
      <!-- 我的服务 -->
      <myserve v-else-if="currentContent === 1" />
      <!-- 我的收藏 -->
      <mycollect v-else-if="currentContent === 3" />
      <!-- 我的评价 -->
      <myassessment v-else-if="currentContent === 4" />
    </div>
  </div>
</template>

<script>
import menus from '@/views/personalCenter/components/LeftMenus'
import basicMsg from '@/views/personalCenter/components/BasicMsg'
import myserve from '@/views/personalCenter/components/Myserve'
import mycollect from '@/views/personalCenter/components/MyCollect'
import myassessment from '@/views/personalCenter/components/MyAssessment'
import OpenPlatformHeader from '@/components/page/OpenPlatformHeader'
export default {
  name: 'perCenter',
  components: {
    OpenPlatformHeader,
    menus,
    basicMsg,
    myserve,
    mycollect,
    myassessment,
  },
  data() {
    return {
      currentContent: 0, //初始化右侧内容基本信息
    }
  },
  methods: {
    /**
     * 子组件点击方法
     * 返回当前索引 后续可拿唯一值对应切换右侧对应内容
     */
    handleSelect(i) {
      this.currentContent = i
    },
  },
}
</script>

<style lang="less" scoped>
#perCenter {
  width: 100%;
  .content {
    display: flex;
    min-width: 1200px;
    max-width: 70%;
    margin: 20px auto;
  }
}
</style>