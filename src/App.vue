<template>
  <v-app
    id="app"
    v-touch="{
      left: () => swipe('Left'),
      right: () => swipe('Right'),
    }"
  >
    <v-main class="wrapper">
      <apod v-if="card === 1" />
      <wheater v-if="card === 2" />
    </v-main>
  </v-app>
</template>

<script>
import apod from "./components/apod";
import wheater from "@/components/wheater";

export default {
  name: "App",

  components: {
    apod,
    wheater,
  },
  data: () => ({
    card: 1,
  }),
  methods: {
    swipe(direction) {
      console.log(direction);
      if (direction === "Right") {
        if (this.card < 2) {
          this.card += 1;
        }
      }
      if (direction === "Left") {
        if (this.card > 1) {
          this.card -= 1;
        }
      }
    },
  },
};
</script>
<style>
html,
body {
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  overflow-x: hidden;
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
