<template>
  <div id="Openfooter">
    <div class="conten">
      <div class="toplink">
        <img src="~@assets/images/red.png" alt="" />
        <div style="cursor: pointer;">开放协议</div>
        <div>|</div>
        <div style="cursor: pointer;">使用手册</div>
        <div>|</div>
        <div style="cursor: pointer;" @click="toHelp">帮助中心</div>
        <div>|</div>
        <div style="cursor: pointer;">版权声明</div>
        <div>|</div>
        <div style="cursor: pointer;">隐私声明</div>
        <div>|</div>
        <div style="cursor: pointer;">网站地图</div>
      </div>
      <div class="contactInformation">
        <div>主办：孝感市政务服务和大数据管理局</div>
        <div>地址：孝感市新桥路1号</div>
        <div>联系电话：0712-2281666</div>
        <div>传真：0712-2281359</div>
      </div>
      <div class="websiteIdentity">
        <div>网站标识码：4209000048</div>
        <div>鄂ICP备05015859号</div>
        <div>鄂公网安备42090202000171</div>
        <div>技术支持：孝感市门户网站管理中心</div>
      </div>
    </div>
    <img class="jiucuo" src="~@assets/images/jiucuo.png" alt="" />
  </div>
</template>
<script>
export default {
  name: 'Openfooter',
  data() {
    return {}
  },
  methods: {
    toHelp() {
      this.$router.push({
        path: '/helpCenter/problemManual'
      })
    }
  }
}
</script>

<style lang="less" scoped>
#Openfooter {
  width: 100%;
  height: 150px;
  color: #fff;
  background-color: #4f5967;
  display: flex;
  justify-content: center;
  position: relative;
  .conten {
    width: 50%;
    height: 100%;
    // background-color: cadetblue;
    .toplink {
      margin-top: 5px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      img {
        width: 64px;
        height: 64px;
      }
    }
    .contactInformation {
      margin-top: 5px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .websiteIdentity {
      margin-top: 15px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
  }
  .jiucuo {
    width: 100px;
    height: 50px;
    position: absolute;
    top: 30%;
    right: 10%;
  }
}
</style>
