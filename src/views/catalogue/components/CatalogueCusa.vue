<template>
  <div id="CatalogueCusa">
    <div class="example">示例数据</div>
    <div>
      <el-table
        ref="multipleTable"
        :data="dataList"
        tooltip-effect="dark"
        style="width: 100%"
        :header-cell-style="{ background: '#f6f6f9', color: '#333333' }"
        @selection-change="handleSelectionChange"
        class="tablemargin"
      >
        <el-table-column label="序号" type="index"> </el-table-column>
        <el-table-column prop="name" label="中文名称" width="180">
          <template slot-scope="{ row }">
            {{ row.name || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="enName" label="英文名称" width="180">
          <template slot-scope="{ row }">
            {{ row.enName || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="dataType" label="数据类型" width="180">
          <template slot-scope="{ row }">
            {{ row.dataType || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="dataLength" label="数据长度" width="180">
          <template slot-scope="{ row }">
            {{ row.dataLength || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="dataDictionary" label="数据字典项" width="180">
          <template slot-scope="{ row }">
            {{ row.dataDictionary || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="defaultValue" label="默认值" width="180">
          <template slot-scope="{ row }">
            {{ row.defaultValue || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="shareProperty" label="共享属性" width="180">
          <template slot-scope="{ row }">
            {{ row.shareProperty || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="sharingCondition" label="共享条件" width="180">
          <template slot-scope="{ row }">
            {{ row.sharingCondition || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="openAttribute" label="开放属性" width="180">
          <template slot-scope="{ row }">
            {{ row.openAttribute || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="nullValue" label="是否主键" width="180">
          <template slot-scope="{ row }">
            {{ row.nullValue === '1' ? '是' : '否' || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="nullValue" label="是否可为空" width="180">
          <template slot-scope="{ row }">
            {{ row.nullValue === '1' ? '是' : '否' || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="dataObject" label="数据对象" width="180">
          <template slot-scope="{ row }">
            {{ row.dataObject || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="dataLevel" label="数据级别" width="180">
          <template slot-scope="{ row }">
            {{ row.dataLevel || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="fieldDescription" label="字段描述" width="180">
          <template slot-scope="{ row }">
            {{ row.fieldDescription || '-' }}
          </template>
        </el-table-column>
      </el-table>
      <div class="fileDownload">文件下载</div>
      <el-table
        class="tab"
        :data="catalogDataDetails.dataList"
        :header-cell-style="{ background: '#f6f6f9', color: '#333333' }"
      >
        <el-table-column label="序号" type="index"> </el-table-column>
        <el-table-column prop="name" label="文件名称">
          <template slot-scope="{ row }">
            {{ row.name || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="engName" label="文件格式">
          <template slot-scope="{ row }">
            {{ row.enName || '-' }}
            <i class="el-icon-download"></i>
          </template>
        </el-table-column>
        <el-table-column prop="dataType" label="文件类型">
          <template slot-scope="{ row }">
            {{ row.dataType || '-' }}
          </template>
        </el-table-column>
        <el-table-column prop="dataLength" label="文件生成时间">
          <template slot-scope="{ row }">
            {{ row.dataLength || '-' }}
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
export default {
  props: ['catalogDataDetails'],
  name: 'CatalogueCusa',
  data() {
    return {
      dataList: ''
    }
  },
  watch: {
    catalogDataDetails(newVal) {
      newVal.updateTime = this.formatTime(newVal.updateTime)
      this.dataList = newVal.dataList
    }
  }
}
</script>

<style lang="less" scoped>
#CatalogueCusa {
}
.example {
  margin-bottom: 10px;
}
.fileDownload {
  margin: 10px 0;
}
</style>
