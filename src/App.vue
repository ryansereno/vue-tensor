<template>
  <div style="display: flex; align-items: center">
    <button @click="stepwiseMultiplication">Multiply Matrices</button>
    <Matrix
      ref="matrixARef"
      :matrix="matrixA"
      :activeRows="matrixAActiveRows"
    />
    <span> X </span>
    <div :style="transformStyle" class="matrix-transition">
      <Matrix
        ref="matrixBRef"
        :matrix="matrixB"
        :activeColumns="matrixBActiveColumns"
        :matrixIsRotated="isRotated"
      />
    </div>
    <Matrix v-if="resultMatrix" :matrix="resultMatrix"></Matrix>
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

const matrixAActiveRows = ref([]);
const matrixBActiveColumns = ref([]);

const isRotated = ref(false);
const resultMatrix = ref(null);

const step = ref(0);

let translateX, translateY, rowHeightA;

const stepwiseMultiplication = () => {
  const rowsA = matrixA.value.length;
  const colsA = matrixA.value[0].length;
  const rowsB = matrixB.value.length;
  const colsB = matrixB.value[0].length;
  //check dimensions
  if (colsA !== rowsB) {
    console.error("Matrix multiplication not possible");
    return;
  }
  const maxSteps = matrixA.value.length + matrixB.value[0].length; // Total steps needed

  if (step.value === 0) {
    //get DOM element locations
    const matrixARect = matrixARef.value.$el.getBoundingClientRect();
    const matrixBRect = matrixBRef.value.$el.getBoundingClientRect();

    //initial movement of MatrixB, positioned above MatrixA and rotated
    translateX = matrixARect.left - matrixBRect.left;
    translateY = matrixARect.top - matrixBRect.top;
    rowHeightA = matrixARect.height / matrixA.value.length;

    //initialize resultMatrix wirh null values
    const result = Array(rowsA)
      .fill(null)
      .map(() => Array(colsB).fill(null));
    resultMatrix.value = result;

    //send cell isRotated state to child matrices
    isRotated.value = true;
  }

  if (step.value < maxSteps) {
    // Determine the current diagonal to calculate
    let diag = step.value - 1;

    // Reset the active rows and columns at the beginning of each step
    matrixAActiveRows.value = [];
    matrixBActiveColumns.value = [];

    // Loop to calculate the values of cells in the current diagonal
    for (let i = 0; i <= diag; i++) {
      let row = i;
      let col = diag - i;

      if (row < rowsA && col < colsB) {
        matrixAActiveRows.value.push(row);
        matrixBActiveColumns.value.push(col);
        resultMatrix.value[row][col] = 0;
        for (let k = 0; k < colsA; k++) {
          resultMatrix.value[row][col] +=
            matrixA.value[row][k] * matrixB.value[k][col];
        }
      }
    }
    //translateX and translateY are variables set during step 0, they are just updated here
    transformStyle.value.transform = `
      translateX(${translateX}px) 
      translateY(${translateY + step.value * rowHeightA}px) 
      rotateZ(-90deg)
    `;
    transformStyle.value.transformOrigin = "top left";

    // Increment the step and set a timeout to call this function again
    step.value++;
    setTimeout(stepwiseMultiplication, 1000);
  }
};
</script>

<style>
.matrix-transition {
  transition: transform 1s, top 1s, left 1s;
}
</style>
