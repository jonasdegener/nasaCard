<template>
  <v-app
    id="app"
    v-touch="{
      left: () => swipe('Left'),
      right: () => swipe('Right'),
    }"
  >
    <div class="jd-help-text">
      <lottie
        id="lottieAni"
        :options="defaultOptions"
        :height="100"
        :width="300"
        @animCreated="handleAnimation"
      />
    </div>
    <v-main class="wrapper">
      <apod v-if="card === 1" />
      <wheater v-if="card === 2" />
    </v-main>
  </v-app>
</template>

<script>
import apod from "./components/apod";
import wheater from "@/components/wheater";
import swipe from "/src/swipe.json";
import Lottie from "vue-lottie";

export default {
  name: "App",

  components: {
    apod,
    wheater,
    Lottie,
  },
  data: () => ({
    card: 1,
    defaultOptions: { animationData: swipe, autoplay: true },
  }),
  methods: {
    handleAnimation: function (anim) {
      this.anim = anim;
    },
    swipe(direction) {
      console.log(direction);
      if (direction === "Right") {
        if (this.card > 1) {
          this.card -= 1;
        }
      }
      if (direction === "Left") {
        if (this.card < 2) {
          this.card += 1;
        }
      }
    },
  },
};
</script>
<style>
.jd-help-text {
  display: flex;
  justify-content: center;
  font-size: 12px;
}
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
