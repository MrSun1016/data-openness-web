<template>
  <div
    v-show="ready"
    class="el-carousel__item"
    :class="{
      'is-active': active,
      'el-carousel__item--card': $parent.type === 'card',
      'is-in-stage': inStage,
      'is-hover': hover,
      'is-animating': animating
    }"
    @click="handleItemClick"
    :style="itemStyle"
  >
    <div v-if="$parent.type === 'card'" v-show="!active" class="el-carousel__mask"></div>
    <slot></slot>
  </div>
</template>

<script>
import { autoprefixer } from 'element-ui/src/utils/util'
// const CARD_SCALE = 0.6;
// 缩放比例
const CARD_SCALE = 0.8
export default {
  name: 'ElCarouselItem',
  props: {
    name: String,
    label: {
      type: [String, Number],
      default: ''
    },
    // 自定义卡片宽度
    cardwidth: {
      type: [String, Number],
      default: '50%'
    }
  },
  data() {
    return {
      hover: false,
      translate: 0,
      scale: 1,
      active: false,
      ready: false,
      inStage: false,
      animating: false,
      parentOffsetWidth: 0 //当前元素父元素宽度
    }
  },

  methods: {
    processIndex(index, activeIndex, length) {
      if (activeIndex === 0 && index === length - 1) {
        return -1
      } else if (activeIndex === length - 1 && index === 0) {
        return length
      } else if (index < activeIndex - 1 && activeIndex - index >= length / 2) {
        return length + 1
      } else if (index > activeIndex + 1 && index - activeIndex >= length / 2) {
        return -2
      }
      return index
    },

    // 卡片化的carousel重新计算位移量
    calcCardTranslate(index, activeIndex) {
      const parentWidth = this.$parent.$el.offsetWidth
      this.parentOffsetWidth = parentWidth
      if (this.inStage) {
        let diffWid = (parseInt(this.cardwidth) - this.parentOffsetWidth / 2) / 1.1
        let changWid = (parentWidth * ((2 - CARD_SCALE) * (index - activeIndex) + 1)) / 4
        //270是基础值，可根据设计图进行修改
        let moveWid =
          changWid > 0
            ? changWid === 360
              ? changWid
              : this.cardwidth == '50%'
              ? changWid + 270
              : changWid + 270 + diffWid
            : this.cardwidth == '50%'
            ? changWid - 270
            : changWid - 270 - diffWid
        return moveWid
      } else if (index < activeIndex) {
        return (-(1 + CARD_SCALE) * parentWidth) / 4
      } else {
        return ((3 + CARD_SCALE) * parentWidth) / 4
      }
    },

    calcTranslate(index, activeIndex, isVertical) {
      const distance = this.$parent.$el[isVertical ? 'offsetHeight' : 'offsetWidth']
      return distance * (index - activeIndex)
    },

    translateItem(index, activeIndex, oldIndex) {
      const parentType = this.$parent.type
      const parentDirection = this.parentDirection
      const length = this.$parent.items.length
      if (parentType !== 'card' && oldIndex !== undefined) {
        this.animating = index === activeIndex || index === oldIndex
      }
      if (index !== activeIndex && length > 2 && this.$parent.loop) {
        index = this.processIndex(index, activeIndex, length)
      }
      if (parentType === 'card') {
        if (parentDirection === 'vertical') {
          console.warn('[Element Warn][Carousel]vertical direction is not supported in card mode')
        }
        this.inStage = Math.round(Math.abs(index - activeIndex)) <= 1
        this.active = index === activeIndex
        this.translate = this.calcCardTranslate(index, activeIndex)
        this.scale = this.active ? 1 : CARD_SCALE
      } else {
        this.active = index === activeIndex
        const isVertical = parentDirection === 'vertical'
        this.translate = this.calcTranslate(index, activeIndex, isVertical)
      }
      this.ready = true
    },

    handleItemClick() {
      const parent = this.$parent
      if (parent && parent.type === 'card') {
        const index = parent.items.indexOf(this)
        parent.setActiveItem(index)
      }
    }
  },

  computed: {
    parentDirection() {
      return this.$parent.direction
    },

    itemStyle() {
      const translateType = this.parentDirection === 'vertical' ? 'translateY' : 'translateX'
      const value = `${translateType}(${this.translate}px) scale(${this.scale})`
      // 添加样式，实现自定义卡片宽度
      const style = {
        width: this.cardwidth,
        transform: value,
        left: this.cardwidth === '50%' ? 0 : `${-(parseInt(this.cardwidth) - this.parentOffsetWidth / 2) / 2}px`
      }
      return autoprefixer(style)
    }
  },

  created() {
    this.$parent && this.$parent.updateItems()
  },

  destroyed() {
    this.$parent && this.$parent.updateItems()
  }
}
</script>
<style scoped>
.el-carousel__item--card {
  border-radius: 12px;
  background: #fff;
}
</style>

