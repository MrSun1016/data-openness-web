<template>
  <div id="ProblemManual">
    <OpenPlatformHeader />
    <div class="center">
      <div class="menu-left">
        <helpMenu :menus="menus" @selectLeft="selectLeft" :selectId="selectId"></helpMenu>
      </div>
      <div class="menu-right">
        <AskedQuestion v-show="selectId == 0"></AskedQuestion>
        <OperatingManual v-show="selectId == 1"></OperatingManual>
      </div>
    </div>
  </div>
</template>
<script>
import OpenPlatformHeader from '@/components/page/OpenPlatformHeader'
import helpMenu from '@/views/helpCenter/components/helpMenu'
import AskedQuestion from '@/views/helpCenter/components/AskedQuestion'
import OperatingManual from '@/views/helpCenter/components/OperatingManual'
export default {
  name: 'ProblemManual',
  components: {
    OpenPlatformHeader,
    helpMenu,
    AskedQuestion,
    OperatingManual
  },
  data() {
    return {
      menus: [
        { id: '1', name: '常见问题' },
        { id: '2', name: '操作手册' }
      ],
      selectId: 0
    }
  },
  methods: {
    selectLeft(selectId) {
      this.selectId = selectId
      // console.log('selectId', selectId)
    }
  }
}
</script>

<style lang="less" scoped>
#ProblemManual {
  width: 100%;
  height: 100%;
  .center {
    width: 90%;
    min-height: calc(95vh - 232px);
    display: flex;
    justify-content: space-around;
    margin: 20px auto;
    .menu-left {
      width: 22%;
    }
    .menu-right {
      width: 78%;
    }
  }
}
</style>
