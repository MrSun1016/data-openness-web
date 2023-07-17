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
  name: 'App',
  provide() {
    return {
      reload: this.reload
    }
  },
  data() {
    return {
      locale: zhCN,
      // isRouterAlive:true
      ratio: null
    }
  },
  activated() {
    let that = this
    enquireScreen(deviceType => {
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
  mounted() {
    this.$nextTick(() => {
      window.addEventListener('resize', () => {
        // 监听浏览器窗口大小改变

        let ratio = 0
        let screen = window.screen
        let ua = navigator.userAgent.toLowerCase()

        if (window.devicePixelRatio !== undefined) {
          ratio = window.devicePixelRatio
        } else if (~ua.indexOf('msie')) {
          if (screen.deviceXDPI && screen.logicalXDPI) {
            ratio = screen.deviceXDPI / screen.logicalXDPI
          }
        } else if (window.outerWidth !== undefined && window.innerWidth !== undefined) {
          ratio = window.outerWidth / window.innerWidth
        }
        console.log('ratio', ratio)
        if (ratio) {
          ratio = Math.round(ratio * 100)
        }
        switch (ratio) {
          case 100:
            this.text = 'text-class'
            break
          case 110:
            this.text = 'text-class2'
            break
          case 125:
            this.text = 'text-class3'
            break
          case 150:
            this.text = 'text-class4'
            break
        }
        this.ratio = ratio
      })
    })
  },
  watch: {
    ratio: {
      handler: function(val) {
        console.log('valapp', val)
        // if (val < 900) {
        //   console.log(val + '屏幕宽度小于900px')
        // } else {
        //   console.log(val + '屏幕宽度大于900px')
        // }
      },
      immediate: true,
      deep: true
    }
  }
}
</script>
<style lang="less" scoped>
#app {
  width: 100%;
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
