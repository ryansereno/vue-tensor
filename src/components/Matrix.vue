<template>
  <table>
    <tr v-for="(row, rowIndex) in matrix" :key="rowIndex">
      <td v-for="(column, colIndex) in row" :key="colIndex">
        {{ column }}
      </td>
    </tr>
  </table>
</template>

<script setup>
import { ref } from 'vue';
import { defineProps } from 'vue';

const props = defineProps(['matrix']);
</script>

<style scoped>
table{
  border-spacing:0;
}
td{
  height:50px;
  width:50px;
  border: 1px solid red;
}
</style>
