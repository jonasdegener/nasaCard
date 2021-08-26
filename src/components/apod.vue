<template>
  <v-container>
    <v-card v-if="nasaResponse" class="mx-auto rounded-xl">
      <div class="d-flex justify-center align-center">
        <v-img
          v-if="pictureUrl.length > 0"
          aspect-ratio="1.7"
          class="white--text align-end"
          :src="pictureUrl"
        >
        </v-img>
        <v-progress-circular
          v-else
          :size="40"
          :width="3"
          color="blue"
          class="mt-5"
          indeterminate
        ></v-progress-circular>
      </div>
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
      </v-card-text>
    </v-card>
    <loading :response="nasaResponse"></loading>
  </v-container>
</template>

<script>
import axios from "axios";
import loading from "@/components/loading";
export default {
  name: "HelloWorld",
  components: {
    loading,
  },

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
          const responseData = response.data;
          console.log(response.data);
          if ("thumb" in responseData) {
            this.pictureUrl = responseData.thumb;
          } else {
            this.pictureUrl = responseData.url;
          }

          this.nasaResponse = responseData;
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
