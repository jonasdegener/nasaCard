<template>
  <div class="wrapper d-flex justify-center">
    <v-progress-circular
      v-if="!response"
      :size="70"
      :width="7"
      color="blue"
      indeterminate
    ></v-progress-circular>
  </div>
</template>
<script>
export default {
  name: "Loading",
  props: { response: Boolean },
};
</script>
<style scoped>
.wrapper {
  width: 100vw;
}
</style>
