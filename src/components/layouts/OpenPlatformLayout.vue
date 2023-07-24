<template>
  <div>
    <PlatformLayout />
    <div id="OpenPlatformLayout">
      <div style="overflow-y: scroll; height: 20%; background-color: #fff">
        <SideMenu />
      </div>
      <div class="content">
        <router-view />
      </div>
    </div>
    <Openfooter></Openfooter>
  </div>
</template>

<script>
import PlatformLayout from '@/components/page/PlatformLayout'
import SideMenu from '@/components/page/SideMenu'
import Openfooter from '@/components/page/Openfooter'
export default {
  name: 'OpenPlatformLayout',
  components: {
    PlatformLayout,
    SideMenu,
    Openfooter
  }
}
</script>

<style lang="less" scoped>
#OpenPlatformLayout {
  width: 90%;
  margin: 20px auto;
  display: flex;
  justify-content: space-between;
  background-color: #fff;
  .content {
    width: 80%;
    padding-left: 24px;
    background: #fff;
  }
}
</style>
