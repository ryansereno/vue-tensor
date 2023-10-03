<template>
  <MatrixMultiplication
    :matrixAData="matrixA"
    :matrixALabels="matrixALabels"
    :matrixBData="matrixB"
    :matrixBLabels="matrixBLabels"
  />
</template>

<script setup>
import { ref } from "vue";
import MatrixMultiplication from "@/components/MatrixMultiplication.vue";

function generateRandomFloat() {
  const number = Math.random() * 10;
  return number.toFixed(1);
}

const rowsA = 8;
const colsA = 10;
const rowsB = 10;
const colsB = 8;

const matrixA = ref([
  [0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1],
  [0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2],
  [0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3],
  [0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4],
  [0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5],
  [0.6, 0.6, 0.6, 0.6, 0.6, 0.6, 0.6, 0.6, 0.6, 0.6],
  [0.7, 0.7, 0.7, 0.7, 0.7, 0.7, 0.7, 0.7, 0.7, 0.7],
  [0.8, 0.8, 0.8, 0.8, 0.8, 0.8, 0.8, 0.8, 0.8, 0.8],
]);

const matrixB = ref([
  [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8],
  [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8],
  [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8],
  [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8],
  [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8],
  [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8],
  [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8],
  [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8],
  [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8],
  [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8],
]);

const matrixALabels = [
  "any",
  "sufficiently",
  "advanced",
  "technology",
  "is",
  "indistinguishable",
  "from",
  "magic",
];
const matrixBLabels = [
  "any",
  "sufficiently",
  "advanced",
  "technology",
  "is",
  "indistinguishable",
  "from",
  "magic",
];
</script>
