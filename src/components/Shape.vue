<template>
  <svg width="50" height="50" viewBox="0 0 50 50" xmlns="http://www.w3.org/2000/svg">
    <g :fill="color">
      <ShapeSquare v-if="shape == 'Square'" />
      <ShapeCircle v-else-if="shape == 'Circle'" />
      <ShapeTriangle v-else-if="shape == 'Triangle'" />
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
  props: ['shape', 'color'],
  data() {
    return {
      iconColor: 'red',
    }
  },
  components: { ShapeSquare, ShapeCircle, ShapeTriangle, ShapeHexagedron },
}
</script>
