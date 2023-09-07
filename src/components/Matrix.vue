<template>
  <table border="1">
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
