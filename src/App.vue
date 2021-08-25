<template>
  <v-app id="app">
    <v-main class="wrapper">
      <HelloWorld />
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
};
</script>
<style>
html,
body {
  height: 100%;
  display: flex;
  justify-content: center;
}
.wrapper {
  display: flex;
  align-items: center;
}
#app {
  font-family: Poppins, serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
