<template>
  <div id="interaction">
    <OpenPlatformHeader />

    <div class="center-max">
      <div class="left-box">
        <CommunicationMenu :menus="menus" @selectLeft="selectLeft" :selectId="selectId" />
      </div>
      <div class="right-box">
        <RightsProtection v-show="selectId == 0" />
        <Problemfeedback v-show="selectId == 1"></Problemfeedback>
        <DataRequirements v-show="selectId == 2"></DataRequirements>
        <DataErrorCorrection v-show="selectId == 3"></DataErrorCorrection>
      </div>
    </div>

    <Openfooter></Openfooter>
    <FloatingIcon></FloatingIcon>
    
  </div>
</template>
<script>
import OpenPlatformHeader from '@/components/page/OpenPlatformHeader'
import Openfooter from '@/components/page/Openfooter'
import CommunicationMenu from '@/views/communication/components/CommunicationMenu'
import RightsProtection from '@/views/communication/components/RightsProtection'
import Problemfeedback from '@/views/communication/components/Problemfeedback'
import DataRequirements from '@/views/communication/components/DataRequirements'
import DataErrorCorrection from '@/views/communication/components/DataErrorCorrection'
import FloatingIcon from '@/components/page/FloatingIcon'

export default {
  name: 'interaction',
  components: {
    OpenPlatformHeader,
    Openfooter,
    CommunicationMenu,
    RightsProtection,
    Problemfeedback,
    DataRequirements,
    DataErrorCorrection,
    FloatingIcon
  },
  data() {
    return {
      menus: [
        { id: '1', name: '权益保护' },
        { id: '2', name: '问题反馈' },
        { id: '3', name: '数据需求' },
        { id: '4', name: '数据纠错' }
        // { id: '5', name: '我的评价' }
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
#interaction {
  background-color: #fff;
  .center-max {
    width: 90%;
    min-height: calc(95vh - 232px);
    display: flex;
    justify-content: space-around;
    margin: 20px auto;
    .left-box {
      width: 22%;
    }
    .right-box {
      width: 78%;
    }
  }
}
</style>
