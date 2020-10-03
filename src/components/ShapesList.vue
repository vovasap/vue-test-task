<template>
  <ul class="table">
    <li v-for="(shape, index) in shapes" :key="index">
      {{ shape }}
    </li>
  </ul>
</template>

<script>
export default {
  // props: ['shapes'],
}
</script>

<style>
.table {
  margin: 0 15px 15px;
  padding: 0;

  list-style: none;
}
</style>
