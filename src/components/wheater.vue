<template>
  <v-container class="jd-overflow">
    <v-card v-if="nasaResponse" class="mx-auto rounded-xl">
      <v-img aspect-ratio="1.2" class="white--text align-end" :src="pictureUrl">
      </v-img>
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
