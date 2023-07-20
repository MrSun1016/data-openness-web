<template>
  <div>
    <div id="CataloguePort">
      <div class="basic-info-content">
        <div class="left-title" style="color: #333333">
          <p>
            <span class="itme-title">接口名称：</span><span>{{ catalogDataDetails.keyAreasType || '-' }}</span>
          </p>

          <p>
            <span class="itme-title">请求方法：</span><span>{{ catalogDataDetails.sourceUnit || '-' }}</span>
          </p>

          <p>
            <span class="itme-title">后端服务类型：</span><span>{{ catalogDataDetails.shareType || '-' }}</span>
          </p>
        </div>
        <div class="left-title" style="color: #333333">
          <p>
            <span class="itme-title">响应类型：</span><span>{{ catalogDataDetails.keyAreasType || '-' }}</span>
          </p>

          <p>
            <span class="itme-title">请求数据类型：</span><span>{{ catalogDataDetails.sourceUnit || '-' }}</span>
          </p>
        </div>
        <div class="left-title leftbutton">
          <el-button type="primary" @click="applicationInterface()">申请接口</el-button>
        </div>
      </div>
      <!-- 请求参数 -->
      <div class="require-params">
        <div style="margin: 14px 0; color: #000">请求参数</div>
        <el-table :data="shareApiInfo.parameterList" :header-cell-style="{ background: '#f6f6f9', color: '#333333' }">
          <el-table-column prop="parameterName" label="参数名称">
            <template slot-scope="{ row }">
              {{ row.parameterName || '-' }}
            </template>
          </el-table-column>
          <!-- <el-table-column prop="required" label="是否必填">
            <template slot-scope="{ row }">
              {{ row.required == '1' ? '否' : '是' || '-' }}
            </template>
          </el-table-column> -->
          <el-table-column prop="required" label="是否必填">
            <template slot-scope="{ row }">
              {{ row.required || '-' }}
            </template>
          </el-table-column>
          <el-table-column prop="parameterType" label="类型">
            <template slot-scope="{ row }">
              {{ row.parameterType || '-' }}
            </template>
          </el-table-column>
          <el-table-column prop="defaultValue" label="默认值">
            <template slot-scope="{ row }">
              {{ row.defaultValue || '-' }}
            </template>
          </el-table-column>
          <el-table-column prop="describetion" label="描述">
            <template slot-scope="{ row }">
              {{ row.describetion || '-' }}
            </template>
          </el-table-column>
          <el-table-column prop="parameterLocation" label="参数位置">
            <template slot-scope="{ row }">
              {{ row.parameterLocation || '-' }}
            </template>
          </el-table-column>
        </el-table>
      </div>
      <!-- 返回参数 -->
      <div class="return-params">
        <div style="margin: 14px 0; color: #000">返回参数</div>
        <el-table
          :data="shareApiInfo.parameteReturnList"
          :header-cell-style="{ background: '#f6f6f9', color: '#333333' }"
        >
          <el-table-column prop="parameterName" label="参数名称">
            <template slot-scope="{ row }">
              {{ row.parameterName || '-' }}
            </template>
          </el-table-column>
          <!-- <el-table-column prop="required" label="是否必填">
            <template slot-scope="{ row }">
              {{ row.required == '1' ? '否' : '是' || '-' }}
            </template>
          </el-table-column> -->
          <el-table-column prop="required" label="是否必填">
            <template slot-scope="{ row }">
              {{ row.required || '-' }}
            </template>
          </el-table-column>
          <el-table-column prop="parameterType" label="类型">
            <template slot-scope="{ row }">
              {{ row.parameterType || '-' }}
            </template>
          </el-table-column>
          <el-table-column prop="defaultValue" label="默认值">
            <template slot-scope="{ row }">
              {{ row.defaultValue || '-' }}
            </template>
          </el-table-column>
          <el-table-column prop="describetion" label="描述">
            <template slot-scope="{ row }">
              {{ row.describetion || '-' }}
            </template>
          </el-table-column>
          <el-table-column prop="parameterLocation" label="参数位置">
            <template slot-scope="{ row }">
              {{ row.parameterLocation || '-' }}
            </template>
          </el-table-column>
        </el-table>
      </div>
      <!-- 请求示例 -->
      <div class="require-example">
        <p style="margin: 24px 0; color: #000000">请求示例</p>
        <el-input type="textarea" :rows="3" v-model="shareApiInfo.requestExample"> </el-input>
      </div>
      <div class="return-example">
        <p style="margin: 24px 0; color: #000000">返回示例</p>
        <el-input type="textarea" :rows="3" v-model="shareApiInfo.returnExample"> </el-input>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['catalogDataDetails', 'shareApiInfo'],
  name: 'CataloguePort',
  data() {
    return {
      textarea: '',
      dataList: [],
      parameterList: [],
      parameteReturnList: [],
      requestExample: '',
      returnExample: ''
    }
  },
  methods: {
    applicationInterface() {
      this.$emit('handeleDrawer')
    }
  },
  watch: {
    catalogDataDetails(newVal) {
      this.dataList = newVal
      if (newVal.apiList != null) {
        this.parameterList = newVal.apiList[0].parameterList
        this.parameteReturnList = newVal.apiList[0].parameteReturnList
        this.requestExample = newVal.apiList[0].requestExample
        this.returnExample = newVal.apiList[0].returnExample
      }
    }
  }
}
</script>

<style lang="less" scoped>
#CataloguePort {
  // width: 100%;
  .basic-info-content {
    display: flex;
    padding: 32px;
    justify-content: space-between;
    .left-title {
      width: 33%;
    }
    .leftbutton {
      display: flex;
      flex-direction: row-reverse;
      align-items: center;
      button {
        width: 100px;
        height: 35px;
        // line-height: 32px;
        background-color: #4888ff;
      }
    }
  }
}
// .require-params {
//   padding: 0 32px;
// }
// .return-params {
//   padding: 0 32px;
// }
</style>
