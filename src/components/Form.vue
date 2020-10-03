<template>
  <form>
    <input type="text" v-model="currentName" placeholder="name" />
    <select v-model="currentColor">
      <option disabled value="">Select a color</option>
      <option v-for="(color, index) in colors" :key="index">
        {{ color }}
      </option>
    </select>
    <select v-model="currentFigure">
      <option disabled value="">Select a figure</option>
      <option v-for="(figure, index) in figures" :key="index">
        {{ figure }}
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
      currentFigure: '',
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
      figures: ['Square', 'Circle', 'Triangle', 'Hexagedron'],
    }
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
