<template>
  <div id="homeStatistics">
    <div class="content-card">
      <div class="card" v-for="cards in cardList" :key="cards.id">
        <img class="card-img" :src="cards.imgUrl"  />
        <div>{{ cards.title }}</div>
        <div class="number">{{cards.number}}个</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'homeStatistics',
  data(){
    return{
        cardList:[
            {id:'1',imgUrl:require("../../../assets/images/actExp.png"),title:"市级部门/区",number:'213'},
            {id:'2',imgUrl:require("../../../assets/images/actExp.png"),title:"开放目录总量",number:'213'},
            {id:'3',imgUrl:require("../../../assets/images/actExp.png"),title:"数据项总量",number:'213'},
            {id:'4',imgUrl:require("../../../assets/images/actExp.png"),title:"开放数据总量",number:'213'},
            {id:'5',imgUrl:require("../../../assets/images/actExp.png"),title:"接口总量",number:'213'},
            {id:'6',imgUrl:require("../../../assets/images/actExp.png"),title:"可下载文件量",number:'213'},
            {id:'7',imgUrl:require("../../../assets/images/actExp.png"),title:"数据下载量",number:'213'},
            {id:'8',imgUrl:require("../../../assets/images/actExp.png"),title:"接口调用量",number:'213'},
            {id:'9',imgUrl:require("../../../assets/images/actExp.png"),title:"平台访问量",number:'213'},
            {id:'10',imgUrl:require("../../../assets/images/actExp.png"),title:"应用成果总量",number:'213'},
        ],
    }
  },
}
</script>

<style lang="less" scoped>
#homeStatistics {
    background: #fff;
    padding: 15px 0;
    margin-top: 18px;
  .content-card {
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
    .card {
      width: 20%;   
      text-align: center;
      margin-bottom: 15px;
      .card-img{
        margin: 8px 0;
      }
      .number{
        color: #1ac3ff;
      }
    }
  }
}
</style>