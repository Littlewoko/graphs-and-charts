<script setup lang="ts">
import { ref, reactive, computed } from "vue";
import { Bar } from "vue-chartjs";
import type { ChartData } from "chart.js";

type movie = { title: string; rating: number };
const movies = ref<movie[]>([]);

fetch("/api.json")
  .then(async (res) => {
    movies.value = await res.json();
  })
  .catch((err) => alert("error accessing json"));

const chartData = computed((): ChartData<"bar"> => {
  return {
    labels: movies.value.map((movie: movie) => movie.title),
    datasets: [
      {
        label: "real",
        // borderColor: "red",
        // borderWidth: { right: 2, left: 1},
        // borderRadius: 5,
        stack:"rating",
        data: movies.value.map((movie: movie) => movie.rating),
      },

      {
        label: "nonsense",
        backgroundColor: ["black"],
        stack:"rating",
        data: movies.value.map((movie: movie) => movie.rating + 1),
      },
    ],
  };
});
</script>

<template>
  <Bar
    v-if="movies.length"
    :data="chartData"
    :options="{
      plugins: {
        legend: { display: true },
      },
      indexAxis: 'y'
    }"
  />
  <!-- <pre>{{ movies }}</pre> -->
</template>

<style>
body {
  padding: 50px;
}
</style>