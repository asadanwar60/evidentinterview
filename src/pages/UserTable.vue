<template>
  <TableData
    :columnName="columnName"
    :sortedData="sortedData"
    @sortAscTable="sortAscTable"
    @sortDscTable="sortDscTable"
    sorting
  >
    <template #rows-Picture="value">
      <img :src="value.rows.picture" />
    </template>
  </TableData>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
import TableData from "@/components/TableData.vue";

const tableData = ref([]);
const sortedData = ref([]);

async function getTableData() {
  const response = await fetch("https://randomuser.me/api/?results=50");
  const responseData = await response.json();
  tableData.value = responseData.results;
  sortedData.value = userData.value;
  sortDscTable("slno");
}

const userData = computed(() => {
  return tableData.value.map((user, index) => ({
    slno: index + 1,
    name: `${user.name?.first} ${user.name?.last}`,
    dob: new Date(user.dob?.date).toLocaleDateString(),
    email: user.email,
    location: `${user.location?.street?.number}, ${user.location?.street?.name}, ${user.location?.city}, ${user.location?.state}, ${user.location?.country}, ${user.location?.postcode}`,
    phone: user.phone,
    picture:
      user.picture?.large || user.picture?.medium || user.picture?.thumbnail,
  }));
});

const columnName = computed(() => {
  return Object.keys(userData.value[0] || {}).map(
    (key) => key.charAt(0).toUpperCase() + key.slice(1)
  );
});

function sortAscTable(sortBy) {
  sortedData.value.sort((a, b) => (a[sortBy] > b[sortBy] ? 1 : -1));
}

function sortDscTable(sortBy) {
  sortedData.value.sort((a, b) => (a[sortBy] < b[sortBy] ? 1 : -1));
}

onMounted(() => {
  getTableData();
});
</script>

<style>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>
