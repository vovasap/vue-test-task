<template>
  <div id="app">
    <div class="field"></div>
    <div class="data-container">
      <Form @addFigure="addFigure" :figures="figures" />
      <FiguresList :figures="figures" />
    </div>
  </div>
</template>

<script>
import Form from '@/components/Form'
import FiguresList from '@/components/FiguresList'
export default {
  name: 'App',
  components: { Form, FiguresList },
  data() {
    return {
      figures: [],
    }
  },
  methods: {
    addFigure(figure) {
      this.figures.push(figure)
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
