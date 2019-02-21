<template>
  <div id="app">
    <MainWindow></MainWindow>
  </div>
</template>

<script>
import MainWindow from "./components/MainWindow.vue";

export default {
  name: "app",
  components: {
    MainWindow
  },
  data() {
    return {
      msg: "Welcome to Your Vue.js App"
    };
  }
};
</script>

<style lang="scss">
</style>
