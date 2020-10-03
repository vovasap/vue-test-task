<template>
  <table class="table">
    <tr>
      <th>Name</th>
      <th>Color</th>
      <th>Shape</th>
    </tr>
    <tr v-for="(shape, index) in shapes" :key="index">
      <td>{{ shape.name }}</td>
      <td>{{ shape.color }}</td>
      <td>{{ shape.shape }}</td>
    </tr>
  </table>
</template>

<script>
export default {
  props: ['shapes'],
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
