<template>
  <div id="app">
    <div class="field">
      <Shape
        v-for="(figure, index) of figures"
        :key="index"
        :figures="figures"
        :figure="figure"
      ></Shape>
    </div>
    <div class="data-container">
      <Form @addFigure="addFigure" :figures="figures" @removeFigure="removeFigure" />
      <FiguresList :figures="figures" />
    </div>
  </div>
</template>

<script>
import Form from '@/components/Form'
import FiguresList from '@/components/FiguresList'
import Shape from '@/components/Shape'
export default {
  name: 'App',
  components: { Form, FiguresList, Shape },
  data() {
    return {
      figures: [],
    }
  },
  methods: {
    addFigure(figure) {
      this.figures.push(figure)
    },
    removeFigure() {
      this.figures = this.figures.filter((figure) => !figure.isActive)
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
  position: relative;
  width: 500px;
  height: 500px;

  border: 1px solid #555;
  background: #eee;
}
</style>
