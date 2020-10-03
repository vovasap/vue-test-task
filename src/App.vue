<template>
  <div id="app">
    <div class="field"></div>
    <div class="data-container">
      <Form @addShape="addShape" />
      <ShapesList :shapes="shapes" />
    </div>
  </div>
</template>

<script>
import Form from '@/components/Form'
import ShapesList from '@/components/ShapesList'
export default {
  name: 'App',
  components: { Form, ShapesList },
  data() {
    return {
      shapes: [],
    }
  },
  methods: {
    addShape(shape) {
      this.shapes.push(shape)
    },
  },
}
</script>

<style>
body {
  margin: 0;
}
#app {
  display: flex;
  justify-content: center;

  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 10px;
}
.field {
  width: 500px;
  height: 500px;

  border: 1px solid #555;
  background: #eee;
}
</style>
