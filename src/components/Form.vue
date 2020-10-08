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
  props: ['figures'],
  data() {
    return {
      hasError: false,
      currentName: '',
      currentColor: '',
      currentShape: '',
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
      shapes: ['Square', 'Circle', 'Triangle', 'Hexagedron'],
      errors: [],
      errorKinds: { name: false, color: false, shape: false },
    }
  },
  created() {
    this.colors.sort()
  },
  methods: {
    createShape() {
      this.validateFormElements()
      if (this.errors.length) {
        return
      }
      this.$emit('addFigure', {
        name: this.currentName,
        color: this.currentColor,
        shape: this.currentShape,
        position: 'static',
        x: 0,
        y: 0,
        isActive: false,
      })
      this.colors = this.colors.filter((color) => color !== this.currentColor)
      this.currentName = ''
      this.currentColor = ''
      this.currentShape = ''
      this.isExistingName = false
    },
    validateFormElements() {
      this.errors = []
      this.errorKinds = {}
      if (
        !this.currentName.trim() ||
        this.figures.findIndex((figure) => figure.name == this.currentName.trim()) >= 0
      ) {
        this.errors.push('Pick another name')
        this.errorKinds.name = true
      }
      if (!this.currentColor) {
        this.errors.push('Select a color')
        this.errorKinds.color = true
      }
      if (!this.currentShape) {
        this.errors.push('Select a shape')
        this.errorKinds.shape = true
      }
      if (this.errors.length) {
        this.hasError = true
      }
      return this.errors
    },
    removeFigure() {
      const activeFigure = this.figures.find((figure) => figure.isActive)
      this.colors.push(activeFigure.color)
      this.colors.sort()
      this.$emit('removeFigure')
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
