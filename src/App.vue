<template>
  <div class="container mt-4">
    <h1 class="mb-4 text-center">סניפי הרשת</h1>
    <StoreTable />
  </div>
</template>

<script>
import StoreTable from "./components/StoreTable.vue";

export default {
  name: "App",
  components: {
    StoreTable,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
