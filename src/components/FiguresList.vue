<template>
  <table class="table">
    <tr class="table-row__header">
      <th>Name</th>
      <th>Color</th>
      <th>Shape</th>
      <th>X</th>
      <th>Y</th>
    </tr>
    <tr
      v-for="(figure, index) in figures"
      :key="index"
      :id="figure.name"
      class="table-row__data"
      :class="{ active: figure.isActive }"
      @click="toggleActive"
    >
      <td>{{ figure.name }}</td>
      <td>{{ figure.color }}</td>
      <td>{{ figure.shape }}</td>
      <td>{{ figure.x + 25 }}</td>
      <td>{{ figure.y + 25 }}</td>
    </tr>
  </table>
</template>

<script>
export default {
  props: ['figures', 'currentFigure'],
  methods: {
    toggleActive(e) {
      const nameSelectedFigure = e.target.parentNode.id

      this.figures.forEach((figure) => {
        if (figure.name !== nameSelectedFigure) {
          figure.zIndex = 0
          figure.isActive = false
        } else {
          figure.zIndex = 1
          figure.isActive = true
          this.$emit('changeCurrentFigure', figure)
        }
      })
    },
  },
}
</script>

<style>
.table {
  width: 100%;
  margin: 0 15px 15px;
  padding: 0;

  border-collapse: collapse;
  list-style: none;
}
.table-row__header {
  border-bottom: 2px solid #000;
}
.table-row__data {
  text-align: center;
  cursor: pointer;
}
td {
  padding: 3px 0;
}
.active {
  background: #ccc;
}
</style>
