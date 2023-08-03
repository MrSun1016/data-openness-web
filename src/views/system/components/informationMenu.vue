<template>
  <div class="information-menu">
    <div id="informationMenu">
      <div class="menus">
        <div
          v-for="(item, index) in menus"
          :key="item.id"
          class="items"
          :class="{ active: index === selectIndex }"
          @click="handleSelect(index)"
        >
          <div class="rectangle" v-if="index === selectIndex"></div>
          <div style="margin: 0 5px">{{ item.name }}</div>
        </div>
      </div>
    </div>
    <informationContent
      :listData="listData"
      ref="infRef"
      :qureyParame="qureyParame"
      :total="total"
      @handleSearch="handleSonSearch"
    />
  </div>
</template>

<script>
import informationContent from '@/views/system/components/informationContent'
import { getInformation } from '@/api/api'
export default {
  name: 'informationMenu',
  components: {
    informationContent,
  },
  props: {
    menus: Array,
    // default: ['', ''],
    require: true,
  },
  data() {
    return {
      total: 0,
      // 请求参数
      qureyParame: {
        informationName: '',
        pageNum: 1,
        pageSize: 10,
        informationType: '1',
      },
      selectIndex: 0,
      listData: [
        {
          informationName: '',
          releaseTime: '',
        },
      ],
    }
  },
  mounted() {
    this.$route.query.currentSelect ? (this.selectIndex = this.$route.query.currentSelect) : this.selectIndex
    this.fetchInformation()
  },
  methods: {
    handleSonSearch(data) {
      this.qureyParame.informationName = data.informationName
      this.fetchInformation()
    },
    fetchInformation() {
      this.$refs.infRef.loading = true
      getInformation(this.qureyParame)
        .then((res) => {
          this.listData = res.body.content
          this.listData.map((item) => {
            item.releaseTime = this.formatTime(item.releaseTime, 'hms')
          })
          this.total = res.body.total
          this.$refs.infRef.loading = false
        })
        .catch(() => {
          this.$refs.infRef.loading = false
        })
    },
    handleSelect(i) {
      this.selectIndex = i
      // this.this.$refs.infRef.fileViewer = false
      this.selectIndex === 0 ? (this.qureyParame.informationType = 1) : (this.qureyParame.informationType = 2)
      this.qureyParame.informationName = ''
      this.qureyParame.pageNum = 1
      this.qureyParame.pageSize = 10
      this.$bus.$emit('handleCloseViewer', false)
      // this.qureyParame.informationType = this.selectIndex = i +1
      this.fetchInformation()
    },
  },
}
</script>

<style lang="less" scoped>
.information-menu {
  width: 100%;
  display: flex;
  flex: 1;
}
#informationMenu {
  width: 30%;
  .menus {
    // width: 280px;
    // margin: 0 auto;
    cursor: pointer;
    border-radius: 5px;
    background: rgba(246, 246, 249);
    // background: #919aa8;
    .items {
      padding-left: 24px;
      display: flex;
      align-items: center;
      height: 52px;
      line-height: 52px;
    }
    .rectangle {
      width: 3px;
      height: 20px;
      display: inline-block;
      background: #fff;
    }
  }
  .active {
    color: #fff;
    background: #4888ff;
    border-radius: 5px 5px 0 0;
  }
}
</style>