<template>
  <div style="display: flex">
    <table v-if="props.rowLabels">
      <tr v-for="(row, rowIndex) in matrixData" :key="rowIndex">
        <td>
          <span class="label">
            {{ props.rowLabels[rowIndex] }}
          </span>
        </td>
      </tr>
    </table>

    <div>
      <table v-if="props.columnLabels">
        <tr>
          <td v-for="(label, labelIdx) in props.columnLabels" :key="labelIdx">
            <span
              :class="{
                rotated: props.matrixIsRotated,
                label,
              }"
            >
              {{ props.columnLabels[labelIdx] }}
            </span>
          </td>
        </tr>
      </table>
      <table>
        <tr v-for="(row, rowIndex) in matrixData" :key="rowIndex">
          <td v-for="(column, colIndex) in row" :key="colIndex">
            <span
              :class="{
                activeRow: isActiveRow(rowIndex),
                activeColumn: isActiveColumn(colIndex),
                rotated: props.matrixIsRotated,
              }"
              :style="{
                backgroundColor:
                  column != null ? getHeatmapColor(column) : 'transparent',
                color:
                  column != null
                    ? getTextColor(getHeatmapColor(column))
                    : 'inherit',
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
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const props = defineProps({
  matrixData: Array,
  activeRows: Array,
  activeColumns: Array,
  matrixIsRotated: Boolean,
  rowLabels: Array,
  columnLabels: Array,
  isHeatMap: Boolean,
});
const isActiveRow = (rowIndex) =>
  props.activeRows && props.activeRows.includes(rowIndex);

const isActiveColumn = (colIndex) =>
  props.activeColumns && props.activeColumns.includes(colIndex);

//get min and max for entire matrix
const matrixMinMax = computed(() => {
  let min = Infinity;
  let max = -Infinity;
  for (const row of props.matrixData) {
    for (const value of row) {
      if (value != null) {
        min = Math.min(min, value);
        max = Math.max(max, value);
      }
    }
  }
  return { min, max };
});

const getHeatmapColor = (value) => {
  if (props.isHeatMap) {
    const { min, max } = matrixMinMax.value;
    const ratio = (value - min) / (max - min);

    const hue = 210 - ratio * (210 - 60); // interpolate between 210 and 60
    const saturation = 100; // keep saturation constant at 100%
    const lightness = 20 + ratio * (50 - 20); // interpolate between 20% and 50%

    return `hsl(${hue}, ${saturation}%, ${lightness}%)`;
  } else {
    return "transparent"; // default color when heatmap is not active
  }
};
const getTextColor = (backgroundColor) => {
  if (backgroundColor === "transparent") return "inherit";

  const match = backgroundColor.match(/\d+/g);
  if (!match) return "black";

  const [r, g, b] = match.map(Number);

  // Calculating perceived luminance
  const luminance = (0.2126 * r + 0.7152 * g + 0.0722 * b) / 255;

  // Determine if the text should be white or black based on the luminance value
  return luminance > 0.5 ? "black" : "white";
};
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
.label {
  inline-size: 50px;
  overflow: visible;
  overflow-wrap: break-word;
}
td > span {
  transition: transform 0.3s;
  display: flex;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

.activeRow {
  transform: scale(0.7) translateX(-16px) translateY(10px);
  color: lightblue !important;
}
.activeColumn.rotated {
  transform: rotateZ(90deg) scale(0.7) translateX(17px) translateY(-13px);
  color: lightblue !important;
}
.rotated:not(.activeColumn) {
  transform: rotateZ(90deg);
}
.inner-cell-math-symbol {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 0.7em;
}
.between-cell-math-symbol {
  position: absolute;
  top: 50%;
  left: 100%;
  transform: translate(-50%, -50%);
  font-size: 0.9em;
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
