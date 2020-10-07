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
      <component :is="currentFigure"></component>
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
  computed: {
    currentFigure() {
      return `Shape${this.figure.shape}`
    },
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
        const newFigurePositionX = this.figure.x + e.pageX - this.oldLeft
        const newFigurePositionY = this.figure.y + e.pageY - this.oldTop
        const maxLeftBound = this.maxW - this.minW - 52
        const maxBottomBound = this.maxH - this.minH - 52
        this.figure.x = this.restrictToBounds(newFigurePositionX, 0, maxLeftBound)
        this.figure.y = this.restrictToBounds(newFigurePositionY, 0, maxBottomBound)
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
