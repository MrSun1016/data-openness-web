<template>
  <div id="perCenter">
    <OpenPlatformHeader />
    <div class="content">
      <menus @handleSelect="handleSelect" />
      <!-- 基本信息 -->
      <basicMsg v-if="(currentContent === 0)" />
      <!-- 我的服务 -->
      <myserve v-else-if="(currentContent === 1)" />
    </div>
  </div>
</template>

<script>
import menus from '@/views/personalCenter/components/LeftMenus'
import basicMsg from '@/views/personalCenter/components/BasicMsg'
import myserve from '@/views/personalCenter/components/Myserve'
import OpenPlatformHeader from '@/components/page/OpenPlatformHeader'
export default {
  name: 'perCenter',
  components: {
    OpenPlatformHeader,
    menus,
    basicMsg,
    myserve,
  },
  data() {
    return {
      currentContent: 0, //初始化右侧内容队友菜单为基本信息
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