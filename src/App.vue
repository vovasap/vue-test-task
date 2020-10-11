<template>
  <div id="app">
    <div class="field">
      <Shape
        v-for="(figure, index) of figures"
        :key="index"
        :figures="figures"
        :figure="figure"
        :currentFigure="currentFigure"
        @changeCurrentFigure="changeCurrentFigure"
        @removeFigure="removeFigure"
      ></Shape>
    </div>
    <div class="data-container">
      <Form
        :figures="figures"
        :colors="colors"
        :currentFigure="currentFigure"
        @addFigure="addFigure"
        @addColor="addColor"
        @removeFigure="removeFigure"
        @updateCurrentFigureProps="updateCurrentFigureProps"
      />
      <FiguresList
        :figures="figures"
        :currentFigure="currentFigure"
        @changeCurrentFigure="changeCurrentFigure"
      />
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
      currentFigure: null,
      colors: [
        'Black',
        'Gray',
        'Silver',
        'White',
        'Fuchsia',
        'Purple',
        'Red',
        'Maroon',
        'Yellow',
        'Olive',
        'Lime',
        'Green',
        'Aqua',
        'Teal',
        'Blue',
        'Navy',
      ],
    }
  },
  created() {
    document.documentElement.addEventListener('click', this.deactivateFigure)
    this.colors.sort()
  },
  methods: {
    addFigure(figure) {
      this.figures.push(figure)
      this.colors = this.colors.filter((color) => color !== figure.color)
    },
    removeFigure(id) {
      if (id) {
        this.figures = this.figures.filter((figure) => figure.id !== id)
      } else {
        this.figures = this.figures.filter((figure) => !figure.isActive)
      }
    },
    deactivateFigure(e) {
      if (
        e.target.className === 'data-container' ||
        e.target.className === 'field' ||
        e.target.id === 'app'
      ) {
        if (this.currentFigure) {
          this.figures.forEach((figure) => {
            if (figure.name === this.currentFigure.name && figure.id !== this.currentFigure.id) {
              this.currentFigure.name = this.currentFigure.color
            }
          })
          this.colors = this.colors.filter((color) => color !== this.currentFigure.color)
          if (this.currentFigure.name.length === 0) {
            this.currentFigure.name = this.currentFigure.color.toLowerCase()
          }
        }
        this.figures.forEach((figure) => (figure.isActive = false))
        this.currentFigure = null
      }
    },
    changeCurrentFigure(figure) {
      if (this.currentFigure) {
        this.colors = this.colors.filter((color) => color !== this.currentFigure.color)
      }
      this.currentFigure = figure
      this.addColor(this.currentFigure.color)
    },
    updateCurrentFigureProps(prop, value) {
      this.currentFigure[prop] = value
    },
    addColor(color) {
      this.colors.push(color)
      this.colors.sort()
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
