<template>
  <v-row class="align-center" align="center" justify="center">
    <v-col class="text-center" align="center" justify="center" cols="12">
      <v-col class="d-flex" cols="12" sm="6">
        <v-select
          v-model="selectedCountry"
          label="Select Country..."
          :items="countries"
          item-text="name"
          return-object
          outlined
        ></v-select>
      </v-col>
      Country: {{this.selectedCountry.name}}
      <h1>Select Height Range</h1>
      <v-row>
        <v-col class="px-4">
          <v-range-slider
            color="black"
            v-model="range"
            :max="max"
            :min="min"
            hide-details
            class="align-center"
          >
            <template v-slot:prepend>
              <v-text-field
                v-model="range[0]"
                class="mt-0 pt-0"
                hide-details
                single-line
                type="number"
                style="width: 60px"
              ></v-text-field>
            </template>
            <template v-slot:append>
              <v-text-field
                v-model="range[1]"
                class="mt-0 pt-0"
                hide-details
                single-line
                type="number"
                style="width: 60px"
              ></v-text-field>
            </template>
          </v-range-slider>
        </v-col>
      </v-row>
      From: {{this.range[0]}}
      To: {{this.range[1]}}
      <v-btn
        class="ma-2"
        outlined
        color="black"
        v-on:click="findHills($event,range,selectedCountry.id)"
      >Search</v-btn>

      <v-btn class="ma-2" outlined color="black" v-on:click="loadDiagram">Show Diagram</v-btn>
      <!-- 
      table
      -->
      <v-simple-table dark>
        <template v-slot:default v-if="isSelected">
          <thead>
            <tr>
              <th class="text-left">Name</th>
              <th class="text-left">Height</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="hill in hills" :key="hill.id">
              <td>{{ hill.name }}</td>
              <td>{{ hill.height }}</td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </v-col>
  </v-row>
</template>

<script>
// @ is an alias to /src

export default {
  data() {
    return {
      slider: 40,
      range: [0, 6000],
      selectedCountry: {},
      isSelected: false,
      min: 0,
      max: 6000,
      countries: [],
      hills: []
    };
  },

  methods: {
    findHills: function($event, range, id) {
      this.isSelected = true;

      if (id == 52 || id == null) {
        this.$http
          .get("http://localhost:8080/hills/range/" + range[0] + "/" + range[1])
          .then(function(data) {
            this.hills = data.body;
          });
      } else {
        this.$http
          .get(
            "http://localhost:8080/hills/range/" +
              range[0] +
              "/" +
              range[1] +
              "/" +
              id
          )
          .then(function(data) {
            this.hills = data.body;
          });
      }
    },
    loadDiagram: function() {
      this.$emit("loadDiagram", this.hills);
    }
  },
  created() {
    this.$http.get("http://localhost:8080/country/").then(function(data) {
      this.countries = data.body;
    });
  }
};
//
</script>

<style scoped>
.v-data-table {
  border-radius: 12px;
  margin-left: 111px;
  margin-right: 111px;
}
</style>