<template>
<div class="flex items-center  min-h-max  p-0 m-0">
  <SearchTab />
</div>
</template>

<script>
import SearchTab from "./components/SearchTab.vue";
export default {
  name: "App",
  components: {
    SearchTab,
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
