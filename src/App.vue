<template>
  <div id="app">
    <div class="field"></div>
    <div class="data-container">
      <Form />
      <div class="table">table</div>
    </div>
  </div>
</template>

<script>
import Form from '@/components/Form'
export default {
  name: 'App',
  components: { Form },
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
  width: 500px;
  height: 500px;

  border: 1px solid #555;
  background: #eee;
}
.table {
  margin: 0 15px 15px;
}
</style>
