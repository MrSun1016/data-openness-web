<template>
  <div id="ServiceDetails">
    <div class="detailsHeader">
      <i class="el-icon-arrow-left" @click="goBack"></i>
      <div class="title">App详情</div>
      <img src="~@assets/images/容器.png" alt="" />
    </div>
    <div class="detailsMax">
      <div class="icon">
        <div>
          <i class="el-icon-star-off"></i>
          <!-- el-icon-share -->
          <span>收藏 (1122)</span>
        </div>
        <div>
          <i class="el-icon-share"></i>
          <span>转发分享</span>
        </div>
      </div>
      <!-- 图标 -->
      <div class="detailstubiao">
        <img class="imagetubiao" src="~@assets/images/图标.png" alt="" />
        <div class="tubiao-name">
          <div class="tubiao-top">
            <div class="namecolor">应用名称：<span>预约挂号</span></div>
            <div class="rate">
              <span>评分:</span>
              <el-rate v-model="value" disabled show-score text-color="#ff9900" score-template="{value}"> </el-rate>
            </div>
          </div>
          <div class="tubiao-bottom">
            <div>创建时间：<span>2023-05-04</span></div>
          </div>
        </div>
      </div>
      <!-- 信息 -->
      <div class="application">
        <div class="information">
          <div>应用类型：<span>IOS & Android</span></div>
          <div>开发者类型：<span>个人</span></div>
        </div>
        <div class="information">
          <div>数据来源：<span>2023-05-04</span></div>
          <div>浏览量：<span>2023-05-04</span></div>
        </div>
        <div class="information">
          <div>所属领域：<span>2023-05-04</span></div>
          <div>开发者名称：<span>2023-05-04</span></div>
        </div>
      </div>
      <!-- 轮播图 -->
      <div class="rotationChart">
        <carousel class="service-advantage" :interval="40000000" type="card" height="520px">
          <carousel-item v-for="(item, index) in imgList" :key="`a3${index}`" cardwidth="240px">
            <img :src="item.src" style="height:100%;width:100%;" alt="app详解图" />
          </carousel-item>
        </carousel>
      </div>
      <!-- 介绍 -->
      <div class="appIntroduction">
        <span class="tltle">应用介绍：</span>
        <span class="apptext"
          >预约挂号app是一款针对全国各地患者推出的挂号预约服务平台。预约挂号app结合平台大数据用户可根据自己的状况选择预约指定医院挂号陪诊服务，再也不用为排着长队还挂不上号烦恼，大大提升就医效率。</span
        >
      </div>
      <!--  -->
      <div class="association">
        <div class="catalog">关联数据目录信息</div>
        <div class="divider"></div>
        <div class="catalogList">
          <div class="catalogitem">孝感市医院信息</div>
          <div class="catalogitem">孝感市医保报销信息</div>
        </div>
      </div>
      <ServiceCommentscoring></ServiceCommentscoring>
    </div>
  </div>
</template>
<script>
import carousel from './carousel/src/main'
import carouselItem from './carousel/src/item'
import ServiceCommentscoring from './ServiceCommentscoring'
export default {
  name: 'ServiceDetails',
  components: {
    carousel,
    carouselItem,
    ServiceCommentscoring
  },
  data() {
    return {
      value: 3.7,
      imgList: [
        {
          src: require('@/assets/images/19_首页.png')
        },
        {
          src: require('@/assets/images/57_聊天.png')
        },
        {
          src: require('@/assets/images/59_通知.png')
        },
        {
          src: require('@/assets/images/61_我的.png')
        }
      ]
    }
  },
  methods: {
    goBack() {
      // this.$router.back()
      this.$emit('isServiceDetails', false)
    }
  }
}
</script>

<style lang="less" scoped>
#ServiceDetails {
  width: 100%;
  .detailsHeader {
    width: 100%;
    height: 60px;
    background-color: #f0f2f4;
    display: flex;
    align-items: center;
    i {
      font-size: 24px;
      cursor: pointer;
      margin: 0 0 0 5px;
      color: black;
    }
    .title {
      font-size: 18px;
      color: #3d3d3d;
      margin: 0 15px 0 5px;
      font-weight: 600;
    }
    img {
      width: 34px;
      height: 34px;
    }
  }
  .detailsMax {
    .icon {
      color: #97a0ad;
      display: flex;
      justify-content: flex-end;
      div {
        margin: 3px 15px 0 0;
        i {
          margin-right: 5px;
          font-size: 18px;
        }
      }
    }
    .detailstubiao {
      display: flex;
      .imagetubiao {
        width: 120px;
        height: 120px;
        margin-right: 30px;
      }
      .tubiao-name {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        .tubiao-top {
          .namecolor {
            font-size: 18px;
            margin-bottom: 10px;
            span {
              color: #4888ff;
            }
          }
          .rate {
            display: flex;
            align-items: center;
            span {
              font-size: 16px;
            }
          }
        }
      }
    }
    .application {
      margin-top: 15px;
      display: flex;
      flex-direction: row;
      .information {
        margin-right: 50px;
        div {
          color: #919aa8;
          margin-bottom: 10px;
          span {
            color: #535d6b;
          }
        }
      }
    }
    .rotationChart {
      margin-top: 50px;
    }
    .appIntroduction {
      margin-top: 50px;
      background-color: #f6f6f9;
      border-radius: 10px;
      padding: 5px 15px;
      .tltle {
        font-size: 15px;
        color: #191c20;
      }
      .apptext {
        color: #191c20;
      }
    }
    .association {
      margin-top: 50px;

      .catalog {
        font-size: 15px;
        color: #191c20;
      }
      .divider {
        margin: 15px 0;
        border-bottom: 2px solid #f6f6f9;
      }
      .catalogList {
        border-bottom: 2px solid #f6f6f9;
        margin-bottom: 50px;
        .catalogitem {
          font-size: 15px;
          color: #4888ff;
          margin-bottom: 15px;
        }
      }
    }
  }
}
</style>
