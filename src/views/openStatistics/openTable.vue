<template>
  <div id="openTable">
    <OpenPlatformHeader />
    <openEchearts class="openEchearts" />
  </div>
</template>
<script>
import OpenPlatformHeader from '@/components/page/OpenPlatformHeader'
import openEchearts from '@/views/openStatistics/components/openEchearts'
export default {
  name: 'openTable',
  components: {
    OpenPlatformHeader,
    openEchearts
  },
  data() {
    return {}
  }
}
</script>

<style lang="less" scoped>
#openTable {
  width: 100%;
  height: 100%;
  .openEchearts {
    width: 100%;
    height: calc(100% - 82px);
  }
}
</style>
