<template>
  <div class="table-container">
    <table v-if="items.length > 0">
      <thead>
        <tr>
          <th v-for="header in headers" :key="header">
            {{ formatHeader(header) }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, index) in items" :key="index">
          <td v-for="header in headers" :key="header">
           {{ formatCellValue(row[header]) }}
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>No data available.</p>
  </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  items: {
    type: Array,
    required: true,
    default: () => [],
  },
});
const formatCellValue = (value) => {
  if (!value || typeof value !== "string") return value;

  const isoDateRegex = /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}.*Z$/;
  
  if (isoDateRegex.test(value)) {
    const date = new Date(value);
    if (!isNaN(date.getTime())) {
      return date.toLocaleString("th-TH", {
        year: "numeric",
        month: "2-digit",
        day: "2-digit",
        hour: "2-digit",
        minute: "2-digit",
        second: "2-digit",
      });
    }
  }
  
  return value;
};
const headers = computed(() => {
  if (props.items.length === 0) return [];
  return Object.keys(props.items[0]);
});

const formatHeader = (text) => {
  return text
    .replace(/([A-Z])/g, " $1")
    .replace(/^./, (str) => str.toUpperCase());
};
</script>

<style scoped>
.table-container {
  width: 100%;       
  overflow-x: auto;  
  -webkit-overflow-scrolling: touch;
  margin-bottom: 1rem;
}

table {
  width: 100%;
  border-collapse: collapse;
  min-width: 600px;  
}

th, td {
  border: 1px solid #ddd;
  padding: 12px 8px; 
  text-align: left;
  white-space: nowrap; 
}

th {
  background-color: #f4f4f4;
}
</style>
