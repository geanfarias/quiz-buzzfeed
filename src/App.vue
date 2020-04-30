<template>
  <div id="app">
    <question />
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";
import question from "./components/questions.vue";

export default {
  name: "App",
  components: {
    question
  }
};
</script>

<style lang="scss">
@charset "utf-8";
@import "../node_modules/bulma/bulma.sass";
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.is-justify {
    justify-content: center;
}
</style>
