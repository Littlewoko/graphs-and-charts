<script setup lang="ts">
import { ref, computed } from "vue";
import { Line } from "vue-chartjs";
import type { ChartData } from "chart.js";

const weatherData = ref();

fetch("https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&current=temperature_2m,wind_speed_10m&hourly=temperature_2m,relative_humidity_2m,wind_speed_10m")
  .then(async (res) => {
    weatherData.value = await res.json();
  })
  .catch((err) => alert("error accessing json"));

const chartData = computed((): ChartData<"line"> => {
  return {
    labels: weatherData.value.hourly.time.map((timestamp : string) => useDateFormat(timestamp, "dddd h aa").value),
    datasets: [
      {
        label: "temperature",
        data: weatherData.value.hourly.temperature_2m,
      },
    ],
  };
});
</script>

<template>
  <Line
  v-if="weatherData"
    :data="chartData"
    :options="{
      plugins: {
        legend: { display: true },
      },
    }"
  />
  <!-- <pre>{{ movies }}</pre> -->
</template>

<style>
body {
  padding: 50px;
}
</style>