<template>
  <div>
    <div id="serviceItem" v-show="!isDetails">
      <div class="dataSearch">
        <el-input class="dataInput" placeholder="请输入关键字" suffix-icon="el-icon-search" v-model="datainput">
        </el-input>
      </div>
      <ServiceTable
        ref="catalogTbaleRef"
        :total="total"
        :catalogueTableData="catalogueTableData"
        :serviceName="serviceName"
        @handleMatterSort="handleMatterSort"
        @isServiceDetails="isServiceDetails"
      />
      <div class="divdata" v-show="catalogueTableData == []"><p>暂无数据</p></div>
      <div class="pagination-box">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="searchParams.pageNum"
          :page-sizes="[10, 20, 30, 50]"
          :page-size="searchParams.pageSize"
          layout="total, sizes, prev, pager, next, jumper"
          background
          :total="total"
        >
        </el-pagination>
      </div>
    </div>
    <ServiceDetails v-show="isDetails" @isServiceDetails="isServiceDetails"></ServiceDetails>
    <FloatingIcon></FloatingIcon>
  </div>
</template>

<script>
import { ServiceMatterspage } from '@/api/api'
import ServiceInquire from '@views/service/components/ServiceInquire'
import ServiceTable from '@views/service/components/ServiceTable'
import ServiceDetails from '@views/service/components/ServiceDetails'
import FloatingIcon from '@/components/page/FloatingIcon'

import { Message } from 'element-ui'
export default {
  name: 'ServiceItem',
  components: {
    ServiceInquire,
    ServiceTable,
    ServiceDetails,
    FloatingIcon
  },
  data() {
    return {
      datainput: '',
      searchParams: {
        orderBy: 'updateTime',
        orderSc: 'desc',
        businessCode: '', //业务办理项编码
        businessName: '', //业务办理项名称
        catalogName: '',
        dataFormat: [], //资源类型
        electronicLicense: [], //是否电子证照
        functionalType: '',
        implementationCode: '', //实施清单编码
        implementationName: '', //实施清单名称
        orderByField: '',
        pageNum: 1,
        pageSize: 10,
        rowStart: 0,
        selections: '',
        // 默认查孝感市
        sourceUnitCode: '', //树节点编码
        updateFrequency: [], //更新周期/频率
        userId: ''
      },
      catalogueTableData: [],
      total: 0,
      serviceName: '事项',
      isDetails: false
    }
  },
  activated() {
    this.searchParams.sourceUnitCode = ''
    this.$bus.$on('fatherReset', _ => {
      this.searchParams.dataFormat = []
      this.searchParams.electronicLicense = []
      this.searchParams.updateFrequency = []
    })
    this.$bus.$on('changeHandleNode', nodeCode => {
      this.searchParams.sourceUnitCode = nodeCode
      this.handleCurrentChange()
    })
    // 默认查孝感市
    // this.searchParams.sourceUnitCode = '孝感市'
    // this.handleCurrentChange()
  },
  beforeDestroy() {
    this.$bus.$off('fatherReset')
    this.$bus.$off('changeHandleNode')
  },
  created() {},
  mounted() {
    this.handleCurrentChange()
  },
  methods: {
    isServiceDetails(val) {
      this.isDetails = val
    },

    handleMatterSort(type, val) {
      this.searchParams.orderBy = type
      this.searchParams.orderSc = val
      this.handleCurrentChange()
    },
    // 每页数量发生改变的监听
    handleSizeChange(pageSize) {
      this.searchParams.pageNum = pageSize
      this.handleCurrentChange()
    },
    // 根据选择加当前页发请求
    handleCurrentChange(page = 1) {
      // 默认查孝感市
      // this.searchParams.sourceUnitCode = '孝感市'
      this.$refs.catalogTbaleRef.tableLoading = true
      this.searchParams.pageNum = page
      ServiceMatterspage(this.searchParams)
        .then(res => {
          if (res.success) {
            this.$refs.catalogTbaleRef.tableLoading = false
            this.catalogueTableData = res.body.content
            this.total = res.body.total
            this.catalogueTableData.forEach(v => {
              if (v.notionalPoolingNum == null) v.notionalPoolingNum = 0
              else v.notionalPoolingNum
              v.releaseTime = this.formatTime(v.releaseTime, 'hms')
              let resDataRange = v.dataRange.indexOf('[')
              if (resDataRange != -1) {
                v.dataRange = JSON.parse(v.dataRange)
                v.dataRange = v.dataRange.flat(Infinity)
                v.dataRange = Array.from(new Set(v.dataRange))
              } else {
                this.$refs.catalogTbaleRef.tableLoading = false
                // Message.error(res.header.message)
              }
            })
          }
        })
        .catch(_ => {
          Message.error(res.header.message)
          this.$refs.catalogTbaleRef.tableLoading = false
        })
    },
    serviceValue(serviceList) {
      console.log(serviceList.catalogName)
      this.searchParams.catalogName = serviceList.catalogName
      this.searchParams.implementationCode = serviceList.implementationCode
      this.searchParams.implementationName = serviceList.implementationName
      this.searchParams.businessCode = serviceList.businessCode
      this.searchParams.businessName = serviceList.businessName
      this.handleCurrentChange()
    },
    servicesCycle(frequencyType) {
      this.searchParams.updateFrequency = frequencyType
      this.handleCurrentChange()
    },
    servicesLicense(electronicType) {
      this.searchParams.electronicLicense = electronicType
      this.handleCurrentChange()
    },
    servicesResources(resourceType) {
      this.searchParams.dataFormat = resourceType
      this.handleCurrentChange()
    }
  }
}
</script>

<style lang="less" scoped>
#serviceItem {
  width: 100%;
  // height: 100%;
  background: rgb(255, 255, 255);
  padding-bottom: 24px;
  .dataSearch {
    width: 100%;
    height: 64px;
    display: flex;
    align-items: center;
    justify-content: space-around;
    background-color: #dde1e7;
    .dataInput {
      width: 95%;
    }
  }
  .divdata {
    width: 100%;
    height: 150px;
    p {
      font-size: 16px;
      color: darkgrey;
      text-align: center;
    }
  }
  .pagination-box {
    text-align: end;
  }
}
</style>
