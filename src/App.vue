<template>
  <MatrixMultiplication :matrixAData="matrixA" :matrixBData="matrixB" />
</template>

<script setup>
import {ref} from 'vue'
import MatrixMultiplication from "@/components/MatrixMultiplication.vue";

const matrixA = ref([
  [7, 1, 1, 3],
  [0, 1, 1, 3],
  [0, 1, 1, 3],
  [4, 5, 5, 6],
  [7, 8, 8, 9],
  [7, 8, 8, 9],
  [7, 8, 8, 9],
]);

const matrixB = ref([
  [1, 0, 0, 1, 2],
  [3, 2, 2, 3, 1],
  [2, 3, 3, 2, 4],
  [1, 0, 0, 1, 2],
]);
</script>
