<template>
  <div>
    <ServeLayout />
    <div id="PlatformServerLayout">
      <div>
        <div style="overflow-y: auto; height: 100%">
          <ServeMenu />
        </div>
      </div>
      <div style="height: 100%;width: 100%;">
        <div class="content">
          <router-view />
        </div>
      </div>
    </div>
    <Openfooter></Openfooter>
  </div>
</template>

<script>
import ServeLayout from '@/components/page/ServeLayout'
import ServeMenu from '@/components/page/ServeMenu'
import Openfooter from '@/components/page/Openfooter'
export default {
  name: 'PlatformServerLayout',
  components: {
    ServeLayout,
    ServeMenu,
    Openfooter
  }
}
</script>

<style lang="less" scoped>
#PlatformServerLayout {
  width: 90%;
  margin: 20px auto;
  display: flex;
  justify-content: space-between;
  background-color: #fff;
  .content {
    width: 100%;
    padding: 0 24px 24px 24px;
    // height:100%;
    // background: #ffffff;
  }
}
</style>
