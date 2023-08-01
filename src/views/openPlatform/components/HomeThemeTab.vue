<template>
  <div id="homethemeTab">
    <div class="content">
      <div class="tabs">
        <div class="tabs-content">
          <div class="bottom-lin">
            <div
              class="detTitle"
              :class="{ active: selectIndex === index }"
              v-for="(tabs, index) in tablList"
              :key="tabs.id"
              @click="handleSelectTab(index)"
            >
              {{ tabs.name }}
            </div>
            <img class="triangle" ref="triangle" src="../../../assets/images/triangle.png" alt="" />
          </div>
        </div>
      </div>
      <!-- 领域 -->
      <div class="info" v-if="selectIndex === 0" :class="{ on: selectIndex === 0 }">
        <div v-for="item in domainList" :key="item.id" class="item">
          <img :src="item.imgUrl" alt="" style="width: 40px" />
          <div class="title">{{ item.title }}</div>
        </div>
      </div>
      <!-- 主题 -->
      <div v-if="selectIndex === 1" :class="{ on: selectIndex === 1 }">
        <img src="../../../assets//images/zhuti.png" alt="" width="100%" />
      </div>
      <!-- 区/部门 -->
      <div v-if="selectIndex === 2" :class="{ on: selectIndex === 2 }">
        <div class="department">
          <div class="detinfo" v-for="item in departmentList" :key="item.id">
            <img :src="item.imgUrl" />
            <div>{{ item.title }}</div>
          </div>
        </div>
        <!-- 查看更多 -->
        <div class="look-more">
          <div class="more">查看更多</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'homethemeTab',
  data() {
    return {
      selectIndex: 1,
      tablList: [
        {
          id: '1',
          name: '领域',
        },
        {
          id: '2',
          name: '主题',
        },
        {
          id: '3',
          name: '区 / 部门',
        },
      ],
      domainList: [
        { id: '1', title: '信用服务', imgUrl: require('../../../assets/images/beian.png') },
        { id: '2', title: '医疗卫生', imgUrl: require('../../../assets/images/Group.png') },
        { id: '3', title: '社保就业', imgUrl: require('../../../assets/images/worker.png') },
        { id: '4', title: '公共安全', imgUrl: require('../../../assets/images/yingjizb.png') },
        { id: '5', title: '城建住房', imgUrl: require('../../../assets/images/Azurehome.png') },
        { id: '6', title: '交通运输', imgUrl: require('../../../assets/images/NotificationHubs.png') },
        { id: '7', title: '教育文化', imgUrl: require('../../../assets/images/Education.png') },
        { id: '8', title: '科技创新', imgUrl: require('../../../assets/images/Subscriptions.png') },
        { id: '9', title: '资源能源', imgUrl: require('../../../assets/images/zhaohui.png') },
        { id: '10', title: '工业农业', imgUrl: require('../../../assets/images/produce.png') },
        { id: '11', title: '财税金融', imgUrl: require('../../../assets/images/baobiao.png') },
        { id: '12', title: '安全生产', imgUrl: require('../../../assets/images/risk.png') },
        { id: '13', title: '市场监督', imgUrl: require('../../../assets/images/duty.png') },
        { id: '14', title: '社会救助', imgUrl: require('../../../assets/images/morning.png') },
        { id: '15', title: '法律服务', imgUrl: require('../../../assets/images/eBooks.png') },
        { id: '16', title: '生活服务', imgUrl: require('../../../assets/images/taizhang.png') },
        { id: '17', title: '气象服务', imgUrl: require('../../../assets/images/AzureDataExplorerClusters.png') },
        { id: '18', title: '地理空间', imgUrl: require('../../../assets/images/Blueprints.png') },
        { id: '19', title: '机构团体', imgUrl: require('../../../assets/images/Identitygovernance.png') },
        { id: '20', title: '商贸流通', imgUrl: require('../../../assets/images/product.png') },
        { id: '21', title: '其他', imgUrl: require('../../../assets/images/shebei.png') },
      ],
      departmentList: [
        { id: '1', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '2', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '3', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '4', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '5', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '6', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '7', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '8', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '9', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '10', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '11', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '12', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '13', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '14', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        { id: '15', imgUrl: require('../../../assets//images/detImg.png'), title: '孝感市消防救援支队' },
        // {id:'16',imgUrl: require('../../../assets//images/detImg.png'),title:"孝感市消防救援支队"}
      ],
    }
  },
  methods: {
    handleSelectTab(i) {
      if (i === 0) {
        this.$refs.triangle.style.left = '34%'
      } else if (i === 1) {
        this.$refs.triangle.style.left = '445px'
      } else if (i === 2) {
        this.$refs.triangle.style.left = '582px'
      }
      this.selectIndex = i
    },
  },
}
</script>

<style lang="less" scoped>
#homethemeTab {
  width: 100%;
  margin: 15px;
  .content {
    position: relative;
    // padding-top: 90px;
    .tabs {
      z-index: 66;
      cursor: pointer;
      .tabs-content {
        // display: flex;
        width: 1200px;
        // padding: 5px;
        border-bottom: 2px solid #eef1f6;
        .bottom-lin {
          position: relative;
          display: flex;
          justify-content: center;
          width: 900px;
          margin: 0px auto;
          border-bottom: 4px solid #1767ff;
          .triangle {
            height: 5px;
            position: absolute;
            bottom: 0;
            transition: all 0.5s;
            // left: 0;
          }
        }
      }

      position: absolute;
      top: 30px;
      left: 50%;
      transform: translate(-50%, -50%);
      .detTitle {
        width: 140px;
        height: 48px;
        line-height: 48px;
        text-align: center;
      }
    }
    .info {
      display: flex;
      flex-wrap: wrap;
      padding-top: 90px;
      .item {
        cursor: pointer;
        display: flex;
        flex-direction: column;
        width: 179px;
        margin: 15px 0;
        align-items: center;
        .title {
          text-align: center;
        }
      }
    }
    .department {
      width: 100%;
      display: flex;
      flex-wrap: wrap;
      padding-top: 90px;
      // justify-content: space-around;
      .detinfo {
        cursor: pointer;
        display: flex;
        align-items: center;
        width: 23%;
        height: 45px;
        margin: 5px 10px;
        background: #f5f7fa;
        box-shadow: 0px 4px 10px 0px rgba(102, 102, 102, 0.2);
        img {
          margin: 0 5px 0 15px;
        }
      }
    }
    .look-more {
      cursor: pointer;
      text-align: center;
      height: 45px;
      .more {
        width: 220px;
        color: #4888ff;
        background: #f5f7fa;
        height: 45px;
        line-height: 45px;
        margin: 8px auto 0 auto;
        box-shadow: 0px 4px 10px 0px rgba(102, 102, 102, 0.2);
      }
    }
  }
  .active {
    color: #fff;
    background: url('../../../assets/images/activeBgimg.png') no-repeat center/100% 100%;
  }
  .on {
    animation: get-opacity 1s ease-in-out forwards;
  }
  @keyframes get-opacity {
    0% {
      opacity: 0;
    }
    100% {
      opacity: 1;
    }
  }
}
</style>