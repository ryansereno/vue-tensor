<template>
  <table>
    <tr v-for="(row, rowIndex) in matrix" :key="rowIndex">
      <td
        v-for="(column, colIndex) in row"
        :key="colIndex"
        :class="{ activeRow: isActiveRow(rowIndex), activeColumn: isActiveColumn(colIndex) }"
      >
        {{ column }}
      </td>
    </tr>
  </table>
</template>

<script setup>
import { ref } from "vue";
import { defineProps } from "vue";

const props = defineProps(["matrix", "activeRows", "activeColumns"]);
const isActiveRow = (rowIndex) =>
  props.activeRows && props.activeRows.includes(rowIndex);
const isActiveColumn = (colIndex) => props.activeColumns && props.activeColumns.includes(colIndex);

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
.activeRow {
  background-color: blue;
}
.activeColumn{
  background-color: yellow;
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
