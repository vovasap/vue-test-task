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
      zIndex: figure.zIndex,
    }"
    @mousedown="grabFigure"
  >
    <linearGradient id="active">
      <stop offset="20%" stop-color="#000" />
      <stop offset="20%" stop-color="#fff" />
      <stop offset="40%" stop-color="#fff" />
      <stop offset="40%" stop-color="#000" />
      <stop offset="60%" stop-color="#000" />
      <stop offset="60%" stop-color="#fff" />
      <stop offset="80%" stop-color="#fff" />
      <stop offset="80%" stop-color="#000" />
    </linearGradient>
    <g :fill="fillColor">
      <component :is="currentShape" :id="figure.id"></component>
    </g>
  </svg>
</template>

<script>
import ShapeSquare from '@/components/shapes/ShapeSquare'
import ShapeCircle from '@/components/shapes/ShapeCircle'
import ShapeTriangle from '@/components/shapes/ShapeTriangle'
import ShapeHexagedron from '@/components/shapes/ShapeHexagedron'
export default {
  props: ['figure', 'figures', 'currentFigure'],
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
      currentElement: null,
    }
  },
  components: { ShapeSquare, ShapeCircle, ShapeTriangle, ShapeHexagedron },
  mounted() {
    if (!this.enableNativeDrag) {
      this.$el.ondragstart = () => false
    }
  },
  computed: {
    currentShape() {
      return `Shape${this.figure.shape}`
    },
    fillColor() {
      return this.figure.isActive ? 'url(#active)' : this.figure.color
    },
  },
  methods: {
    grabFigure(e) {
      this.currentElement = e.target.closest('svg')
      this.dragging = true
      const { top, left, bottom, right } = this.$el.parentNode.getBoundingClientRect()

      this.minW = left
      this.minH = top
      this.maxW = right
      this.maxH = bottom

      this.oldLeft = e.pageX
      this.oldTop = e.pageY

      this.figures.forEach((figure) => {
        figure.isActive = false
        figure.zIndex = 0
      })
      this.figure.isActive = true
      this.figure.zIndex = 1
      this.$emit('changeCurrentFigure', this.figure)

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

        const centerFigurePositionX = newFigurePositionX + this.minW + 25
        const centerFigurePositionY = newFigurePositionY + this.minH + 25
        this.getResultColllision(centerFigurePositionX, centerFigurePositionY)
      }
      this.oldLeft = e.pageX
      this.oldTop = e.pageY
    },
    getResultColllision(positionX, positionY) {
      this.currentElement.style.visibility = 'hidden'
      let elementBelow = document.elementFromPoint(positionX, positionY)
      if (
        elementBelow.hasAttribute('data-cornersStrength') &&
        elementBelow.getAttribute('data-cornersStrength') < this.figure.cornersStrength
      ) {
        document.documentElement.dispatchEvent(new Event('mouseup'))
        this.$emit('removeFigure', elementBelow.id)
      } else if (
        elementBelow.hasAttribute('data-cornersStrength') &&
        elementBelow.getAttribute('data-cornersStrength') > this.figure.cornersStrength
      ) {
        document.documentElement.dispatchEvent(new Event('mouseup'))
        this.$emit('removeFigure', this.figure.id)
      }
      this.currentElement.style.visibility = 'visible'
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
