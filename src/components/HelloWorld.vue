<template>
  <v-container>
    <v-card v-if="nasaResponse" class="mx-auto rounded-xl">
      <v-img
        aspect-ratio="1.7"
        class="white--text align-end"
        :src="nasaResponse.thumbnail_url"
      >
      </v-img>

      <v-card-text class="text--primary">
        <v-expansion-panels class="rounded-xl jd-pannel my-3">
          <v-expansion-panel>
            <v-expansion-panel-header>
              {{ nasaResponse.title }}
            </v-expansion-panel-header>
            <v-expansion-panel-content>
              {{ nasaResponse.explanation }}
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-card-text>
    </v-card>
    <v-progress-circular
      v-if="!nasaResponse"
      :size="70"
      :width="7"
      color="blue"
      indeterminate
    ></v-progress-circular>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",

  data: () => ({
    pictureUrl: "",
    nasaResponse: null,
  }),
  mounted() {
    this.getapod();
  },
  methods: {
    getapod() {
      axios
        .get(
          "https://api.nasa.gov/planetary/apod?api_key=n1pHbdRcIGmowU8HOEbnb3iQwW89beCbfJnoAk0L&thumbs=true"
        )
        .then((response) => {
          console.log(response.data);
          this.nasaResponse = response.data;
        });
    },
  },
};
</script>
<style>
.jd-pannel {
  box-shadow: rgba(127, 127, 127, 0.25) 0 25px 50px -12px;
}
</style>
