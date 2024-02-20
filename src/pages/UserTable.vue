<template>
  <TableData :columnName="columnName" :sortedData="sortedData">
    <template #col-Slno="value">
      {{ value.column }}
      <button @click="sortTable('slno')">sort</button>
    </template>
    <template #col-Name="value">
      {{ value.column }}
      <button @click="sortTable('name')">sort</button>
    </template>
    <template #col-Dob="value">
      {{ value.column }}
      <button @click="sortTable('dob')">sort</button>
    </template>
    <template #col-Email="value">
      {{ value.column }}
      <button @click="sortTable('email')">sort</button>
    </template>
    <template #col-Location="value">
      {{ value.column }}
      <button @click="sortTable('location')">sort</button>
    </template>
    <template #col-Phone="value">
      {{ value.column }}
      <button @click="sortTable('phone')">sort</button>
    </template>
    <template #col-Picture="value">{{ value.column }}</template>
    <template #rows-Slno="value">{{ value.rows.slno }}</template>
    <template #rows-Name="value">{{ value.rows.name }}</template>
    <template #rows-Dob="value">{{ value.rows.dob }}</template>
    <template #rows-Email="value">{{ value.rows.email }}</template>
    <template #rows-Location="value">{{ value.rows.location }}</template>
    <template #rows-Phone="value">{{ value.rows.phone }}</template>
    <template #rows-Picture="value"><img :src="value.rows.picture" /></template>
  </TableData>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
import TableData from "@/components/TableData.vue";

const tableData = ref([]);
const sortedData = ref([]);
const sortedbyDesc = ref(true);

async function getTableData() {
  const response = await fetch("https://randomuser.me/api/?results=50");
  const responseData = await response.json();
  tableData.value = responseData.results;
  sortedData.value = userData.value;
  sortTable("slno");
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

function sortTable(sortBy) {
  if (sortedbyDesc.value) {
    sortedData.value.sort((a, b) => (a[sortBy] < b[sortBy] ? 1 : -1));
    sortedbyDesc.value = false;
  } else {
    sortedData.value.sort((a, b) => (a[sortBy] > b[sortBy] ? 1 : -1));
    sortedbyDesc.value = true;
  }
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
