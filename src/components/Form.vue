<template>
  <form @submit.prevent="createShape">
    <input
      :class="{ 'form-warning': errorKinds.name }"
      type="text"
      v-model="currentName"
      placeholder="name"
    />
    <select :class="{ 'form-warning': errorKinds.color }" v-model="currentColor">
      <option disabled value="">Select a color</option>
      <option v-for="(color, index) in colors" :key="index">
        {{ color }}
      </option>
    </select>
    <select :class="{ 'form-warning': errorKinds.shape }" v-model="currentShape">
      <option disabled value="">Select a shape</option>
      <option v-for="(shape, index) in shapes" :key="index">
        {{ shape }}
      </option>
    </select>
    <div class="text-warning" v-if="hasError">
      <p v-for="(error, index) in errors" :key="index">
        {{ errors.length > 1 ? index + 1 + '. ' : null }} {{ error }}
      </p>
    </div>
    <div class="buttons">
      <button type="submit">Add</button>
      <button @click.prevent="removeFigure">Delete</button>
    </div>
  </form>
</template>

<script>
export default {
  props: ['colors', 'figures', 'currentFigure'],
  data() {
    return {
      hasError: false,
      name: '',
      color: '',
      shape: '',
      shapes: ['Circle', 'Triangle', 'Square', 'Hexagedron'],
      errors: [],
      errorKinds: { name: false, color: false, shape: false },
      position: '',
      zIndex: 0,
      x: 0,
      y: 0,
      isActive: false,
      id: '',
      cornersStrength: 0,
    }
  },
  computed: {
    currentName: {
      get() {
        if (this.currentFigure) {
          return this.currentFigure.name
        } else {
          return this.name
        }
      },
      set(value) {
        if (this.currentFigure) {
          this.$emit('updateCurrentFigureProps', 'name', value)
        } else {
          this.name = value
        }
      },
    },
    currentColor: {
      get() {
        if (this.currentFigure) {
          return this.currentFigure.color
        } else {
          if (this.colors.find((color) => color === this.color)) {
            return this.color
          } else {
            return ''
          }
        }
      },
      set(value) {
        if (this.currentFigure) {
          this.$emit('updateCurrentFigureProps', 'color', value)
          this.color = value
        } else {
          if (value) {
            this.color = value
          }
        }
      },
    },
    currentShape: {
      get() {
        if (this.currentFigure) {
          return this.currentFigure.shape
        } else {
          return this.shape
        }
      },
      set(value) {
        if (this.currentFigure) {
          this.$emit('updateCurrentFigureProps', 'shape', value)
          this.$emit('updateCurrentFigureProps', 'cornersStrength', this.shapes.indexOf(value))
          this.shape = value
        } else {
          this.shape = value
        }
      },
    },
  },
  methods: {
    createShape() {
      this.validateFormElements()
      if (this.errors.length) {
        return
      }
      const [startX, startY] = this.getCoordsStartingFigure()
      this.$emit('addFigure', {
        name: this.name,
        color: this.color,
        shape: this.shape,
        position: 'absolute',
        zIndex: 0,
        x: startX,
        y: startY,
        isActive: false,
        id: 'f' + new Date().getTime(),
        cornersStrength: this.shapes.indexOf(this.shape),
      })
      this.name = ''
      this.color = ''
      this.shape = ''
    },
    getCoordsStartingFigure() {
      const columnNum = this.figures.length < 8 ? 0 : 1
      const coordX = columnNum * 50
      const rowNum = this.figures.length % 7
      const coordY = rowNum * 50

      return [coordX, coordY]
    },
    validateFormElements() {
      this.errors = []
      this.errorKinds = {}
      if (
        !this.name.trim() ||
        this.figures.findIndex((figure) => figure.name == this.name.trim()) >= 0
      ) {
        this.errors.push('Pick another name')
        this.errorKinds.name = true
      }
      if (!this.color) {
        this.errors.push('Select a color')
        this.errorKinds.color = true
      }
      if (!this.shape) {
        this.errors.push('Select a shape')
        this.errorKinds.shape = true
      }
      if (this.errors.length) {
        this.hasError = true
      }
      return this.errors
    },
    removeFigure() {
      this.currentName = ''
      this.currentColor = ''
      this.currentShape = ''
      const activeFigure = this.figures.find((figure) => figure.isActive)
      if (activeFigure) {
        this.$emit('removeFigure')
      }
    },
  },
}
</script>

<style>
form {
  margin: 0 15px 15px;
  display: flex;
  flex-direction: column; /* max-width: 300px; */
}
input,
select {
  margin-bottom: 10px;
  padding: 5px 10px;

  border: 1px solid #ddd;
  border-radius: 5px;
}
select {
  appearance: none;
}
.buttons {
  display: flex;
  justify-content: space-evenly;
}
button {
  width: 120px;
  margin: 5px;
}
.text-warning {
  margin: 0 0 5px;

  text-align: center;
  font-size: 12px;
  color: #f00;
}
.form-warning {
  border: 1px solid #f00;
}
</style>
