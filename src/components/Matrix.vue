<template>
  <div style="display: flex">
    <!--<table>
    <tr v-for="(row, rowIndex) in matrixData" :key="rowIndex">
      <td v-if="props.rowLabels">{{ props.rowLabels[rowIndex] }}</td>
    </tr>
  </table>-->
    <table>
      <tr v-for="(row, rowIndex) in matrixData" :key="rowIndex">
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
          <span
            class="inner-cell-math-symbol"
            :style="{ opacity: isActiveRow(rowIndex) ? 1 : 0 }"
          >
            ×
          </span>
          <span
            class="between-cell-math-symbol"
            :style="{
              opacity:
                isActiveRow(rowIndex) && colIndex < row.length - 1 ? 1 : 0,
            }"
          >
            +
          </span>
        </td>
      </tr>
    </table>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { defineProps } from "vue";

const props = defineProps([
  "matrixData",
  "activeRows",
  "activeColumns",
  "matrixIsRotated",
  "rowLabels",
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
  position: relative;
  max-width: 50px;
  min-width: 50px;
  height: 50px;
}
td > span {
  transition: transform 0.3s;
  display: flex;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  justify-content: center;
  align-items: center;
  overflow:hidden;
}

.activeRow {
  transform: scale(0.7) translateX(-16px) translateY(10px);
}
.activeColumn:not(.rotated) {
  transform: scale(0.7) translateX(17px) translateY(-13px);
}
.activeColumn.rotated {
  transform: rotateZ(90deg) scale(0.7) translateX(17px) translateY(-13px);
}
.rotated:not(.activeColumn) {
  transform: rotateZ(90deg);
}
.inner-cell-math-symbol {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 0.8em;
}
.between-cell-math-symbol {
  position: absolute;
  top: 50%;
  left: 100%;
  transform: translate(-50%, -50%);
  font-size: 0.8em;
  color: red;
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
