<template>
  <div style="display: flex; flex-direction: column; align-items: center">
    <div
      style="
        display: flex;
        align-items: center;
        position: relative;
        padding: 1rem;
      "
    >
      <Matrix
        ref="matrixARef"
        :matrixData="matrixAData"
        :activeRows="matrixAActiveRows"
        :rowLabels="matrixALabels"
      />
      <span v-if="!resultMatrix" class="matrix-math-operator"> X </span>
      <span v-if="resultMatrix" class="matrix-math-operator"> = </span>
      <div :style="transformStyle" class="matrix-transition">
        <Matrix
          ref="matrixBRef"
          :matrixData="matrixBData"
          :activeColumns="matrixBActiveColumns"
          :matrixIsRotated="isRotated"
          :columnLabels="matrixBLabels"
        />
      </div>
      <div :style="resultMatrixStyle">
        <Matrix
          v-if="resultMatrix"
          :matrixData="resultMatrix"
          :isHeatMap="true"
        ></Matrix>
      </div>
    </div>
    <button style="max-width: 100px" @click="stepwiseMultiplication">
      Multiply Matrices
    </button>
    <button style="max-width: 100px" @click="resetFunction">Reset</button>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Matrix from "@/components/Matrix.vue";
const { matrixAData, matrixBData, matrixALabels, matrixBLabels } = defineProps([
  "matrixAData",
  "matrixBData",
  "matrixALabels",
  "matrixBLabels",
]);
const transformStyle = ref({});

const matrixARef = ref(null);
const matrixBRef = ref(null);
const resultMatrixStyle = ref({});

const matrixAActiveRows = ref([]);
const matrixBActiveColumns = ref([]);

const isRotated = ref(false);
const resultMatrix = ref(null);

const step = ref(0);

const delay = 400;

let translateX, translateY, rowHeightA;

const stepwiseMultiplication = () => {
  const rowsA = matrixAData.length;
  const colsA = matrixAData[0].length;
  const rowsB = matrixBData.length;
  const colsB = matrixBData[0].length;
  //check dimensions
  if (colsA !== rowsB) {
    console.error("Matrix multiplication not possible");
    return;
  }
  const maxSteps = matrixAData.length + matrixBData[0].length; // Total steps needed

  if (step.value === 0) {
    //get DOM element locations
    const matrixARect = matrixARef.value.$el.getBoundingClientRect();
    const matrixBRect = matrixBRef.value.$el.getBoundingClientRect();

    //initial movement of MatrixB, positioned above MatrixA and rotated
    translateY = matrixARect.top - matrixBRect.top;
    rowHeightA = matrixARect.height / matrixAData.length;
    translateX = matrixARect.left - matrixBRect.left;

    //initialize resultMatrix with null values
    const result = Array(rowsA)
      .fill(null)
      .map(() => Array(colsB).fill(null));
    resultMatrix.value = result;

    //send cell isRotated state to child matrices
    isRotated.value = true;
    resultMatrixStyle.value = {
      transform: `translateX(-${matrixBRect.width}px)`,
    };
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
            matrixAData[row][k] * matrixBData[k][col];
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
    setTimeout(stepwiseMultiplication, delay);
  }
};

const resetFunction = () =>{
  step.value = 0
  transformStyle.value = {}
  resultMatrix.value = null
  isRotated.value = false
  matrixAActiveRows = []
  matrixBActiveColumns = []
}
</script>

<style>
.matrix-transition {
  transition: transform 0.4s, top 0.4s, left 0.4s;
}
.matrix-math-operator {
  font-size: 2em;
  margin-left: 1em;
  margin-right: 1em;
}
</style>
