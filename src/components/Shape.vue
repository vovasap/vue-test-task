<template>
  <svg
    width="50"
    height="50"
    viewBox="0 0 50 50"
    xmlns="http://www.w3.org/2000/svg"
    :style="{
      position: figure.position,
      top: figure.y + 'px',
      left: figure.x + 'px',
    }"
    @mousedown="grabFigure"
  >
    <g :fill="figure.color">
      <ShapeSquare v-if="figure.shape == 'Square'" />
      <ShapeCircle v-else-if="figure.shape == 'Circle'" />
      <ShapeTriangle v-else-if="figure.shape == 'Triangle'" />
      <ShapeHexagedron v-else />
    </g>
  </svg>
</template>

<script>
import ShapeSquare from '@/components/shapes/ShapeSquare'
import ShapeCircle from '@/components/shapes/ShapeCircle'
import ShapeTriangle from '@/components/shapes/ShapeTriangle'
import ShapeHexagedron from '@/components/shapes/ShapeHexagedron'
export default {
  props: ['figure'],
  data() {
    return {
      dragging: false,
      oldLeft: this.figure.x,
      oldTop: this.figure.y,
      minW: null,
      minH: null,
      maxW: null,
      maxH: null,
      enableNativeDrag: false,
    }
  },
  components: { ShapeSquare, ShapeCircle, ShapeTriangle, ShapeHexagedron },
  mounted() {
    if (!this.enableNativeDrag) {
      this.$el.ondragstart = () => false
    }
  },
  methods: {
    grabFigure(e) {
      this.dragging = true
      this.figure.position = 'absolute'
      const { top, left, bottom, right } = this.$el.parentNode.getBoundingClientRect()

      this.minW = left
      this.minH = top
      this.maxW = right
      this.maxH = bottom

      this.oldLeft = e.pageX
      this.oldTop = e.pageY

      document.documentElement.addEventListener('mousemove', this.dragFigure)
      document.documentElement.addEventListener('mouseup', this.dropFigure)
    },
    dragFigure(e) {
      if (this.dragging) {
        this.figure.x = this.restrictToBounds(
          this.figure.x + e.pageX - this.oldLeft,
          0,
          this.maxW - this.minW - 52
        )
        this.figure.y = this.restrictToBounds(
          this.figure.y + e.pageY - this.oldTop,
          0,
          this.maxH - this.minH - 52
        )
      }
      this.oldLeft = e.pageX
      this.oldTop = e.pageY
    },
    dropFigure() {
      this.dragging = false
    },
    restrictToBounds(value, min, max) {
      if (value < min) {
        return min
      } else if (value > max) {
        return max
      } else {
        return value
      }
    },
  },
}
</script>
