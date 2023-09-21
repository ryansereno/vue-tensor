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

const matrixA = ref(
  Array.from({ length: rowsA }, () =>
    Array.from({ length: colsA }, () => generateRandomFloat())
  )
);

const matrixB = ref(
  Array.from({ length: rowsB }, () =>
    Array.from({ length: colsB }, () => generateRandomFloat())
  )
);

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
