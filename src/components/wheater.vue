<template>
  <v-container class="jd-overflow">
    <v-card v-if="nasaResponse" class="mx-4 rounded-xl">
      <div class="d-flex justify-center align-center">
        <v-img
          v-if="pictureUrl.length > 0"
          aspect-ratio="1.2"
          class="white--text align-end rounded-xl"
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
      <v-card-subtitle class="pb-0 mb-1">
        If no image exists, the date will be adjusted.
      </v-card-subtitle>

      <v-card-text class="text--primary d-flex justify-center">
        <v-date-picker
          v-model="date"
          flat
          color="black"
          min="2018-01-01"
          :max="currentDate"
        ></v-date-picker>
      </v-card-text>
    </v-card>
    <loading :response="nasaResponse"></loading>
  </v-container>
</template>

<script>
import loading from "@/components/loading";
import axios from "axios";

export default {
  name: "HelloWorld",
  components: {
    loading,
  },
  data: () => ({
    pictureUrl: "",
    nasaResponse: null,
    retry: false,
    currentDate: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    date: "2021-07-26",
    dateFormat: "2021/07/26",
  }),
  watch: {
    // whenever question changes, this function will run
    // eslint-disable-next-line no-unused-vars
    date: function (newDate, oldDate) {
      this.dateFormat = this.date.replaceAll("-", "/");
      this.getapod();
    },
  },
  mounted() {
    this.currentDate = this.formateDate(this.currentDate, -1);
    this.date = this.currentDate;
    this.dateFormat = this.date.replaceAll("-", "/");
    this.getapod();
  },
  methods: {
    formateDate(date, n) {
      const d = new Date(date);
      d.setDate(d.getDate() + n);
      return d.toISOString().split("T")[0];
    },
    getapod() {
      axios
        .get(
          "https://api.nasa.gov/EPIC/api/enhanced/date/" +
            this.date +
            "?api_key=n1pHbdRcIGmowU8HOEbnb3iQwW89beCbfJnoAk0L"
        )
        .then((response) => {
          this.pictureUrl =
            "https://api.nasa.gov/EPIC/archive/enhanced/" +
            this.dateFormat +
            "/png/" +
            response.data[0].image +
            ".png?api_key=n1pHbdRcIGmowU8HOEbnb3iQwW89beCbfJnoAk0L";
          this.retry = false;
          this.nasaResponse = response.data[0];
        })
        .catch(
          // eslint-disable-next-line no-unused-vars
          (err) => {
            if (this.currentDate === this.date || this.retry) {
              this.retry = true;
              this.date = this.formateDate(this.date, -1);
            } else {
              this.date = this.formateDate(this.date, 1);
            }
          }
        );
    },
  },
};
</script>
<style>
.jd-pannel {
  box-shadow: rgba(127, 127, 127, 0.25) 0 25px 50px -12px;
}
.jd-overflow {
  overflow-y: hidden;
}
</style>
