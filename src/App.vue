<template>
  <div id="app">
    <div class="field"></div>

    <div class="data-container">
      <form>
        <input type="text" placeholder="name" />
        <input type="text" placeholder="color" />
        <input type="text" placeholder="figure" />
        <div class="buttons">
          <button>Add</button>
          <button>Delete</button>
        </div>
      </form>

      <div class="table">table</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {},
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
form {
  margin: 0 15px 15px;
  display: flex;
  flex-direction: column; /* max-width: 300px; */
}
input {
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

.table {
  margin: 0 15px 15px;
}
</style>
