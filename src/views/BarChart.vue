<template>
  <div>
    <svg />

    <ul>
      <li v-for="item in issues" :key="item.id">
        {{item.name}}
        {{item.height}}
      </li>
    </ul>
  </div>
</template>

<script>
import * as d3 from "d3";
import _ from "lodash";

export default {
  props: ["issues"],

  data() {
    return {
      chart: null,
      temp: []
    };
  },
  watch: {
    issues() {
      if (this.chart != null) this.chart.remove();
      this.renderChart();
    }
  },
  methods: {
    renderChart() {
      const margin = 60;
      const svg_width = 1000;
      const svg_height = 600;
      const chart_width = 1000 - 2 * margin;
      const chart_height = 600 - 2 * margin;

      const svg = d3
        .select("svg")
        .attr("width", svg_width)
        .attr("height", svg_height);

      this.chart = svg
        .append("g")
        .attr("transform", `translate(${margin}, ${margin})`);

      const yScale = d3
        .scaleLinear()
        .range([chart_height, 0])
        .domain([0, _.maxBy(this.issues, "issues").issues]);

      this.chart
        .append("g")
        .call(d3.axisLeft(yScale).ticks(_.maxBy(this.issues, "issues").issues));

      const xScale = d3
        .scaleBand()
        .range([0, chart_width])
        .domain(issues_val.map(s => s.name))
        .padding(0.2);

      this.chart
        .append("g")
        .attr("transform", `translate(0, ${chart_height})`)
        .call(d3.axisBottom(xScale));

      const barGroups = this.chart
        .selectAll("rect")
        .data(issues_val)
        .enter();

      barGroups
        .append("rect")
        .attr("class", "bar")
        .attr("x", g => xScale(g.name))
        .attr("y", g => yScale(g.height))
        .attr("height", g => chart_height - yScale(g.height))
        .attr("width", xScale.bandwidth());

      svg
        .append("text")
        .attr("class", "label")
        .attr("x", -(chart_height / 2) - margin)
        .attr("y", margin / 2.4)
        .attr("transform", "rotate(-90)")
        .attr("text-anchor", "middle")
        .text("Height");

      svg
        .append("text")
        .attr("class", "label")
        .attr("x", chart_width / 2 + margin)
        .attr("y", chart_height + margin * 1.7)
        .attr("text-anchor", "middle")
        .text("Name");

      svg
        .append("text")
        .attr("class", "title")
        .attr("x", chart_width / 2 + margin)
        .attr("y", 40)
        .attr("text-anchor", "middle")
        .text("Selected Hills Height");
    }
  }
};
</script>