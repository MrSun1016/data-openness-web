<template>
  <a-config-provider :locale="locale">
    <div id="app">
      <keep-alive>
        <router-view v-if="$route.meta.keepAlive" />
      </keep-alive>
      <router-view v-if="!$route.meta.keepAlive"></router-view>
    </div>
  </a-config-provider>
</template>
<script>
import zhCN from 'ant-design-vue/lib/locale-provider/zh_CN'
import enquireScreen from '@/utils/device'

export default {
  name:'App',
  provide(){
      return{
          reload:this.reload
      }
    },
  data() {
    return {
      locale: zhCN,
      // isRouterAlive:true
    }
  },
  activated(){
    let that = this
    enquireScreen((deviceType) => {
      // tablet
      if (deviceType === 0) {
        that.$store.commit('TOGGLE_DEVICE', 'mobile')
        that.$store.dispatch('setSidebar', false)
      }
      // mobile
      else if (deviceType === 1) {
        that.$store.commit('TOGGLE_DEVICE', 'mobile')
        that.$store.dispatch('setSidebar', false)
      } else {
        that.$store.commit('TOGGLE_DEVICE', 'desktop')
        that.$store.dispatch('setSidebar', true)
      }
    })
  },
  created() {
    // let that = this
    // enquireScreen((deviceType) => {
    //   // tablet
    //   if (deviceType === 0) {
    //     that.$store.commit('TOGGLE_DEVICE', 'mobile')
    //     that.$store.dispatch('setSidebar', false)
    //   }
    //   // mobile
    //   else if (deviceType === 1) {
    //     that.$store.commit('TOGGLE_DEVICE', 'mobile')
    //     that.$store.dispatch('setSidebar', false)
    //   } else {
    //     that.$store.commit('TOGGLE_DEVICE', 'desktop')
    //     that.$store.dispatch('setSidebar', true)
    //   }
    // })
  },
}
</script>
<style lang="less" scoped>
#app {
  height: 100%;
  // filter: grayscale(100%)
}
/deep/.ant-table-tbody > tr:nth-child(2n) {
  background: #fafafa;
}
/deep/ .ant-table-thead {
  th {
    // background-color: #E6F3FD !important;;
  }
}
</style>