<template>
  <table class="table">
    <tr>
      <th>Name</th>
      <th>Color</th>
      <th>Shape</th>
    </tr>
    <tr v-for="(figure, index) in figures" :key="index">
      <td>{{ figure.name }}</td>
      <td>{{ figure.color }}</td>
      <td>{{ figure.shape }}</td>
    </tr>
  </table>
</template>

<script>
export default {
  props: ['figures'],
}
</script>

<style>
.table {
  width: 100%;
  margin: 0 15px 15px;
  padding: 0;

  list-style: none;
}
tr {
  text-align: center;
}
</style>
