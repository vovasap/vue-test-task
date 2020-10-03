<template>
  <ul class="table">
    <li v-for="(figure, index) in figures" :key="index">
      {{ figure }}
    </li>
  </ul>
</template>

<script>
export default {
  // props: ['figures'],
}
</script>

<style>
.table {
  margin: 0 15px 15px;
  padding: 0;

  list-style: none;
}
</style>
