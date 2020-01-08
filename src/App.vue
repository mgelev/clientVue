<template>
  <v-app>
    <v-content>
      <Home @loadDiagram="createDiagram" />

      <!-- <ul>
        <li v-for="hill in dataHills" :key="hill.id">{{hill.name}}</li>
      </ul>-->

      <BarChart title="Bar Chart" xKey="name" yKey="height" :issues="dataHills" />
    </v-content>
  </v-app>
</template>

<script>
import Home from "./views/Home";
import BarChart from "./views/BarChart";

export default {
  name: "App",

  dataHills: [],
  components: {
    Home,
    BarChart
  },

  data() {
    return {
      dataHills: []
    };
  },
  methods: {
    createDiagram(data) {
      this.dataHills = JSON.parse(JSON.stringify(data));
      window.console.log(this.dataHills);
    }
  }
};
</script>
