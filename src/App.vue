<template>
<div class="container">
<div class="row mt-5">
  <div class="col">
    <h1 class="text-center">COVID-19 DATA</h1>
  </div>
</div>
<div class="row mt-5" v-if="arrPositive.length > 0">
  <div class="col">
    <h2 class="text-center">Positive</h2>
    <line-chart
      :chartData="arrPositive"
      :options="chartOptions"
      :chartColors="positiveChartColors"
      label="Positive"
    />
  </div>
</div>

<div class="row mt-5" v-if="arrRecovered.length > 0">
  <div class="col">
    <h2 class="text-center">Recovered</h2>
    <line-chart
      :chartData="arrRecovered"
      :options="chartOptions"
      :chartColors="recoveredColors"
      label="Recovered"
    />
  </div>
</div>

<div class="row mt-5 mb-5">
  <div class="col">
    <h2 class="text-center">Deaths</h2>
    <line-chart
      v-if="arrDeaths.length > 0"
      :chartData="arrDeaths"
      :options="chartOptions"
      :chartColors="deathColors"
      label="Deaths"
    />
  </div>
</div>
</div>
</template>

<script>
import axios from "axios";
import moment from "moment";

import LineChart from "./components/LineChart";

export default {
components: {
LineChart
},
data() {
return {
  arrPositive: [],
  positiveChartColors: {
    borderColor: "#077187",
    pointBorderColor: "#0E1428",
    pointBackgroundColor: "#AFD6AC",
    backgroundColor: "#74A57F"
  },
  arrRecovered: [],
  recoveredColors: {
    borderColor: "#4E5E66",
    pointBorderColor: "#4E5E66",
    pointBackgroundColor: "#31E981",
    backgroundColor: "#31E981"
  },
  arrDeaths: [],
  deathColors: {
    borderColor: "#E06D06",
    pointBorderColor: "#E06D06",
    pointBackgroundColor: "#402A2C",
    backgroundColor: "#402A2C"
  },
  chartOptions: {
    responsive: true,
    maintainAspectRatio: false
  }
};
},
async created() {
const { data } = await axios.get("https://covidtracking.com/api/us/daily");

data.forEach(d => {
  const date = moment(d.date, "YYYYMMDD").format("MM/DD");
  const {
    positive,        
    recovered,
    death
  } = d;

  this.arrPositive.push({ date, total: positive });
  this.arrRecovered.push({ date, total: recovered });
  this.arrDeaths.push({ date, total: death });
});
}
};
</script>
<style>
</style>
