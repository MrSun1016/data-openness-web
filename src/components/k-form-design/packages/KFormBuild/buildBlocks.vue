<template>
  <!-- 标签页布局 -->
  <a-tabs
    v-if="record.type === 'tabs'"
    class="grid-row"
    :default-active-key="0"
    :tabBarGutter="record.options.tabBarGutter"
    :type="record.options.type"
    :size="record.options.size"
    :tabPosition="record.options.tabPosition"
    :animated="record.options.animated"
  >
    <a-tab-pane
      v-for="(tabItem, index) in record.columns"
      :key="index"
      :tab="tabItem.label"
    >
      <buildBlocks
        ref="nestedComponents"
        @handleReset="$emit('handleReset')"
        @change="handleChange"
        v-for="item in tabItem.list"
        :disabled="disabled"
        :dynamicData="dynamicData"
        :dictOption="dictOption"
        :key="item.key"
        :record="item"
        :dynamicObject="dynamicObject"
        :formConfig="formConfig"
        :config="config"
        :required="required"
        :preview="preview"
      />
    </a-tab-pane>
  </a-tabs>
  <!-- 栅格布局 -->
  <a-row
    v-else-if="record.type === 'grid'"
    class="grid-row"
    :gutter="record.options.gutter"
  >
    <a-col
      class="grid-col"
      v-for="(colItem, idnex) in record.columns"
      :key="idnex"
      :span="colItem.span || 0"
    >
      <buildBlocks
        ref="nestedComponents"
        @handleReset="$emit('handleReset')"
        @change="handleChange"
        v-for="item in colItem.list"
        :disabled="disabled"
        :dynamicData="dynamicData"
        :dictOption="dictOption"
        :dynamicObject="dynamicObject"
        :key="item.key"
        :record="item"
        :formConfig="formConfig"
        :config="config"
        :required="required"
        :preview="preview"
      />
    </a-col>
  </a-row>
  <!-- 卡片布局 -->
  <a-card
    v-else-if="record.type === 'card'"
    class="grid-row"
    :title="record.label"
  >
    <buildBlocks
      ref="nestedComponents"
      @handleReset="$emit('handleReset')"
      @change="handleChange"
      v-for="item in record.list"
      :disabled="disabled"
      :dynamicData="dynamicData"
      :dictOption="dictOption"
      :dynamicObject="dynamicObject"
      :key="item.key"
      :record="item"
      :formConfig="formConfig"
      :config="config"
      :required="required"
      :preview="preview"
    />
  </a-card>
  <!-- 表格布局 -->
  <table
    v-else-if="record.type === 'table'"
    class="kk-table-9136076486841527"
    :class="{
      bright: record.options.bright,
      small: record.options.small,
      bordered: record.options.bordered
    }"
    :style="record.options.customStyle"
  >
    <tr v-for="(trItem, trIndex) in record.trs" :key="trIndex">
      <td
        class="table-td"
        v-for="(tdItem, tdIndex) in trItem.tds"
        :key="tdIndex"
        :colspan="tdItem.colspan"
        :rowspan="tdItem.rowspan"
      >
        <buildBlocks
          ref="nestedComponents"
          @handleReset="$emit('handleReset')"
          @change="handleChange"
          v-for="item in tdItem.list"
          :disabled="disabled"
          :dynamicData="dynamicData"
          :dictOption="dictOption"
          :dynamicObject="dynamicObject"
          :key="item.key"
          :record="item"
          :formConfig="formConfig"
          :config="config"
          :required="required"
          :preview="preview"
        />
      </td>
    </tr>
  </table>
  
  <KFormItem
    v-else
    ref="nestedComponents"
    @handleReset="$emit('handleReset')"
    @change="handleChange"
    :disabled="disabled"
    :dynamicData="dynamicData"
    :dictOption="dictOption"
    :key="record.key"
    :record="record"
    :dynamicObject="dynamicObject"
    :formConfig="formConfig"
    :config="config"
    :required="required"
    :preview="preview"
  >
  
  </KFormItem>
</template>
<script>
/*
 * author kcz
 * date 2019-11-20
 */
import KFormItem from '../KFormItem/index'
export default {
  name: 'BuildBlocks',
  props: {
    record: {
      type: Object,
      required: true
    },
    formConfig: {
      type: Object,
      required: true
    },
    config: {
      type: Object,
      default: () => ({})
    },
    dynamicData: {
      type: Object,
      required: true
    },
    dictOption: {
      type: Object,
      default: () => ({})
    },
    disabled: {
      type: Boolean,
      default: false
    },
    dynamicObject: {
      type: Object,
      default: () => {}
    },
    required:{
      type:Array,
      default:()=>[]
    },
    preview: {
      type: Boolean,
      default: false
    },
    
  },
  components: {
    KFormItem
  },
  methods: {
    validationSubform() {
      // 验证动态表格
      let nestedComponents = this.$refs.nestedComponents
      if (
        typeof nestedComponents === 'object' &&
        nestedComponents instanceof Array
      ) {
        for (let i = 0; nestedComponents.length > i; i++) {
          if (!nestedComponents[i].validationSubform()) {
            return false
          }
        }
        return true
      } else if (typeof nestedComponents !== 'undefined') {
        return nestedComponents.validationSubform()
      } else {
        return true
      }
    },
    handleChange(value, key) {
      this.$emit('change', value, key)
    }
  }
}
</script>
