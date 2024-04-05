<script setup lang="ts">
import { ref, computed } from "vue";
import { Doughnut } from "vue-chartjs";
import type { ChartData } from "chart.js";

const repos = [
    "vuejs/core", 
    "facebook/react", 
    "angular/angular", 
    "sveltejs/svelte", 
    "solidjs/solid"
];

const repoData = ref<any[]>([]);

repos.forEach((repo) => {
    fetch(`https://api.github.com/repos/${repo}`)
    .then(async (res) => {
        const repo = await res.json();
        repoData.value.push(repo);
    })
});

const chartData = computed((): ChartData<"doughnut"> => {
  return {
    labels: repos,
    datasets: [
        {
            backgroundColor: ["green", "blue", "red", "yellow", "black"], 
            label: "Stars", 
            data: repoData.value.map((repo : any) => repo.stargazers_count)
        }
    ]
  };
});
</script>

<template>
  <Doughnut
    v-if="repoData.length"
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