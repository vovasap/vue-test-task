<template>
  <form @submit.prevent="createShape">
    <input type="text" v-model="currentName" placeholder="name" />
    <select v-model="currentColor">
      <option disabled value="">Select a color</option>
      <option v-for="(color, index) in colors" :key="index">
        {{ color }}
      </option>
    </select>
    <select v-model="currentShape">
      <option disabled value="">Select a shape</option>
      <option v-for="(shape, index) in shapes" :key="index">
        {{ shape }}
      </option>
    </select>
    <div class="buttons">
      <button type="submit">Add</button>
      <button>Delete</button>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
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
    }
  },
  methods: {
    createShape() {
      this.$emit('addFigure', {
        name: this.currentName,
        color: this.currentColor,
        shape: this.currentShape,
      })
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
}
.buttons {
  display: flex;
  justify-content: space-evenly;
}
button {
  width: 120px;
  margin: 5px;
}
</style>
