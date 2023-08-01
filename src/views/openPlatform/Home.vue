<template>
  <div id="openPlatformHome">
    <div>
      <OpenPlatformHeader />
      <homeBanner />
    </div>
    <div class="home-content">
      <!-- 主题tab -->
      <HomeThemeTab/>
      <homeHotList />
      <informationCard />
      <Openfooter />
    </div>
  </div>
</template>

<script>
import OpenPlatformHeader from '@/components/page/OpenPlatformHeader'
import homeBanner from '@/views/openPlatform/components/HomeBanner'
import informationCard from './components/InformationCard'
import homeHotList from './components/HomeHotList'
import HomeThemeTab from './components/HomeThemeTab'
import Openfooter from '@/components/page/Openfooter'
export default {
  name: 'openPlatformHome',
  components: {
    OpenPlatformHeader,
    homeBanner,
    informationCard,
    HomeThemeTab,
    homeHotList,
    Openfooter,
  },
}
</script>

<style lang="less" scoped>
#openPlatformHome {
  // display: flex;
  // flex-direction: column;
  height: 100%;
  overflow-x: hidden;
  // overflow: hidden;
  // padding-top: 0.1px;
  // background: #f0f2f5;
  // .home-content {
  //   width: 100%;
  //   // flex: 1;
  //   // overflow: scroll;
  // }
}
</style>