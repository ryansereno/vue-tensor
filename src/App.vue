<template>
  <div style="display: flex">
    <button @click="multiplyMatrices">Multiply Matrices</button>
    <Matrix :matrix="matrixA" />
    <span> X </span>
    <div
      :class="['matrix-transition', { 'matrix-transformed': isTransformed }]"
    >
      <Matrix :matrix="matrixB" />
    </div>
    <Matrix v-if="false" :matrix="resultMatrix" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import Matrix from "./components/Matrix.vue"; // adjust path as necessary

const matrixA = ref([
  [0.42, 1.28, 3.54],
  [0.42, 1.28, 3.54],
  [0.42, 1.28, 3.54],
  [4.67, 5.82, 6.19],
  [7.21, 8.34, 9.48],
]);

const matrixB = ref([
  [1.42, 0.28, 2.54],
  [3.67, 2.82, 1.19],
  [2.21, 3.34, 4.48],
]);

const isTransformed = ref(false);
const resultMatrix = ref(null);

const multiplyMatrices = () => {
  const rowsA = matrixA.value.length;
  const colsA = matrixA.value[0].length;
  const rowsB = matrixB.value.length;
  const colsB = matrixB.value[0].length;

  if (colsA !== rowsB) {
    console.error("Matrix multiplication not possible");
    return;
  }

  const result = Array(rowsA)
    .fill(0)
    .map((row) => Array(colsB).fill(0));

  for (let i = 0; i < rowsA; i++) {
    for (let j = 0; j < colsB; j++) {
      for (let k = 0; k < colsA; k++) {
        result[i][j] += matrixA.value[i][k] * matrixB.value[k][j];
      }
    }
  }

  resultMatrix.value = result;
  isTransformed.value = true;
};
</script>

<style>
.matrix-transition,
.matrix-transition td{
  transition: transform 1s, top 1s, left 1s;
}
.matrix-transformed {
  transform: translateX(90px) translateY(-250px) rotateZ(-90deg);
  transform-origin: left bottom;
}
.matrix-transformed td {
  transform: rotateZ(90deg);
  transform-origin: center center;
}
</style>
