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

const rowsA = 11;
const colsA = 4;
const rowsB = 4;
const colsB = 11;

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
  "silence",
  "is",
  "the",
  "language",
  "of",
  "god",
  "all",
  "else",
  "is",
  "poor",
  "translation",
];
const matrixBLabels = [
  "silence", 
  "is", 
  "the", 
  "language", 
  "of", 
  "god", 
  "all", 
  "else", 
  "is", 
  "poor", 
  "translation"
];
</script>
