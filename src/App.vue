<style scoped>
div.app {
  max-width: 1000px;
  margin: auto;
  font-family: sans-serif;
}
</style>

<template>
  <div class="app">
    <NavBar />

    <br />

    <Main />
  </div>
</template>

<script>
import NavBar from "./components/NavBar";
import Main from "./components/Main";

export default {
  components: {
    Main,
    NavBar,
  },
};
</script>
