<template>
  <table>
    <tr v-for="(row, rowIndex) in matrix" :key="rowIndex">
      <td v-for="(column, colIndex) in row" :key="colIndex">
        <span
          :class="{
            activeRow: isActiveRow(rowIndex),
            activeColumn: isActiveColumn(colIndex),
            rotated: matrixIsRotated(),
          }"
        >
          {{ column }}
        </span>
      </td>
    </tr>
  </table>
</template>

<script setup>
import { ref } from "vue";
import { defineProps } from "vue";

const props = defineProps([
  "matrix",
  "activeRows",
  "activeColumns",
  "matrixIsRotated",
]);
const isActiveRow = (rowIndex) =>
  props.activeRows && props.activeRows.includes(rowIndex);
const isActiveColumn = (colIndex) =>
  props.activeColumns && props.activeColumns.includes(colIndex);
const matrixIsRotated = () => props.matrixIsRotated;
</script>

<style scoped>
table {
  border-spacing: 0;
}
td {
  max-width: 50px;
  min-width: 50px;
  height: 50px;
  overflow: hidden;
  border: 1px solid gray;
}
td > span {
  display: flex;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  justify-content: center;
  align-items: center;
}
.activeRow,
.activeColumn,
.rotated {
  transition: transform 0.3s; /* Adjust duration as needed */
}

.activeRow {
  transform: scale(0.55) translateX(-16px) translateY(10px);
}
.activeColumn:not(.rotated) {
  transform: scale(0.55) translateX(17px) translateY(-13px);
}
.activeColumn.rotated {
  transform: rotateZ(90deg) scale(0.55) translateX(17px) translateY(-13px);
}
.rotated:not(.activeColumn) {
  transform: rotateZ(90deg);
}
table tr:first-child td:first-child {
  border-top-left-radius: 5px;
}
table tr:first-child td:last-child {
  border-top-right-radius: 5px;
}

table tr:last-child td:first-child {
  border-bottom-left-radius: 5px;
}
table tr:last-child td:last-child {
  border-bottom-right-radius: 5px;
}
</style>
