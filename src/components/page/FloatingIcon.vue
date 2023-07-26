<template>
  <div id="FloatingIcon">
    <div class="collect">
      <img src="~@assets/images/收藏 (2).png" alt="" />
      <div>我的收藏</div>
    </div>
    <div class="collect">
      <img src="~@assets/images/问题反馈.png" alt="" />
      <div>问题反馈</div>
    </div>
    <div class="collect">
      <img src="~@assets/images/权益保护.png" alt="" />
      <div>权益保护</div>
    </div>
    <div class="collect">
      <img src="~@assets/images/数据需求.png" alt="" />
      <div>数据需求</div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'FloatingIcon',
  data() {
    return {}
  }
}
</script>

<style lang="less" scoped>
#FloatingIcon {
  width: 68px;
  height: 260px;
  position: fixed;
  top: 50%;
  right: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  .collect {
    width: 58px;
    height: 58px;
    color: #fff;
    background-color: #618afb;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    img {
      width: 24px;
      height: 24px;
    }
    div {
      font-size: 12px;
      cursor: pointer;
    }
  }
}
</style>
