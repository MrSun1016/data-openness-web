<template>
  <div id="home">
    <div v-show="!flag">
      <homeHeader />
      <homeHCard />
      <HomeAgency />
      <HomeCharts />
    </div>
    <HomeMine />
  </div>
</template>

<script>
import homeHeader from './components/HomeHeader.vue'
import homeHCard from './components/HomeCard.vue'
import HomeAgency from './components/HomeAgency.vue'
import HomeMine from './components/HomeMine.vue'
import HomeCharts from './components/HomeCharts.vue'
export default {
  name: 'home',
  components: {
    homeHeader,
    homeHCard,
    HomeAgency,
    HomeMine,
    HomeCharts
  },
  data(){
    return{
      flag:false
    }
  },
  activated(){
    this.flag = false
  },
  mounted(){
    this.$bus.$on('handleComponents',isshowHaredetails=>{
     this.flag = isshowHaredetails
    })
  },
   //解绑事件
   beforeDestroy() {
    this.$bus.$off('handleComponents')
  },
}
</script>

<style lang="less" scoped>
</style>