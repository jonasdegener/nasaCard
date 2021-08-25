<template>
  <v-container>
    <v-card v-if="nasaResponse" class="mx-auto rounded-xl">
      <v-img
        aspect-ratio="1.7"
        class="white--text align-end"
        :src="nasaResponse.thumbnail_url"
      >
      </v-img>
      <v-card-subtitle class="pb-0 mb-1 text-center">
        Here you get a daily picture + description on a theme of Nasa.
      </v-card-subtitle>

      <v-card-text class="text--primary">
        <v-expansion-panels class="rounded-xl jd-pannel my-3" popout>
          <v-expansion-panel>
            <v-expansion-panel-header>
              {{ nasaResponse.title }}
            </v-expansion-panel-header>
            <v-expansion-panel-content>
              {{ nasaResponse.explanation }}
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
        <div class="d-flex justify-center">
          <a
            class="jd-url text-center"
            :href="nasaResponse.url"
            target="_blank"
          >
            Source or video
          </a>
        </div>
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
.jd-url {
  font-size: 8px;
}
</style>
