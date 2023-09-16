<template>
  <div style="display: flex; align-items: center">
    <button @click="multiplyMatrices">Multiply Matrices</button>
    <Matrix ref="matrixARef" :matrix="matrixA" />
    <span> X </span>
    <div :style="transformStyle" class="matrix-transition">
      <Matrix ref="matrixBRef" :matrix="matrixB" />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Matrix from "./components/Matrix.vue"; // adjust path as necessary

const matrixA = ref([
  [0.42, 1.28, 1.28, 3.54],
  [0.42, 1.28, 1.28, 3.54],
  [0.42, 1.28, 1.28, 3.54],
  [4.67, 5.82, 5.82, 6.19],
  [7.21, 8.34, 8.34, 9.48],
]);

const matrixB = ref([
  [1.42, 0.28, 2.54],
  [3.67, 2.82, 1.19],
  [2.21, 3.34, 4.48],
  [1.42, 0.28, 2.54],
]);
const transformStyle = ref({});

const matrixARef = ref(null);
const matrixBRef = ref(null);

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
  setTransformStyle();
};
const setTransformStyle = () => {
  const matrixAEl = matrixARef.value.$el;
  const matrixBEl = matrixBRef.value.$el;

  const matrixARect = matrixAEl.getBoundingClientRect();
  const matrixBRect = matrixBEl.getBoundingClientRect();

  // Calculate the necessary transform values
  const translateX = matrixARect.left - matrixBRect.left;
  const translateY = matrixARect.top - matrixBRect.top;

  const rowHeightA = matrixARect.height / matrixA.value.length;

  let step = 0;
  const moveStepwise = () => {
    if (step < matrixA.value.length + matrixB.value[0].length + 1) {
      transformStyle.value.transform = `
        translateX(${translateX}px) 
        translateY(${translateY + step * rowHeightA}px) 
        rotateZ(-90deg)
      `;
      transformStyle.value.transformOrigin = "top left";
      step++;
      setTimeout(moveStepwise, 800);
    }
  };
  moveStepwise();
};
</script>

<style>
.matrix-transition,
.matrix-transition td {
  transition: transform 1s, top 1s, left 1s;
}
.matrix-transformed {
  transform-origin: left bottom;
}
/*.matrix-transformed td {
  transform: rotateZ(90deg);
  transform-origin: center center;
}*/
</style>
