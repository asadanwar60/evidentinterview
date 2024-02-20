<template>
  <TableData :columnName="columnName" :sortedData="sortedData">
    <template #col-Slno="value">
      {{ value.column }}
      <button @click="sortAscTable('slno')">Asc</button>
      <button @click="sortDscTable('slno')">Dsc</button>
    </template>
    <template #col-Name="value">
      {{ value.column }}
      <button @click="sortAscTable('name')">Asc</button>
      <button @click="sortDscTable('name')">Dsc</button>
    </template>
    <template #col-Dob="value">
      {{ value.column }}
      <button @click="sortAscTable('dob')">Asc</button>
      <button @click="sortDscTable('dob')">Dsc</button>
    </template>
    <template #col-Email="value">
      {{ value.column }}
      <button @click="sortAscTable('email')">Asc</button>
      <button @click="sortDscTable('email')">Dsc</button>
    </template>
    <template #col-Location="value">
      {{ value.column }}
      <button @click="sortAscTable('location')">Asc</button>
      <button @click="sortDscTable('location')">Dsc</button>
    </template>
    <template #col-Phone="value">
      {{ value.column }}
      <button @click="sortAscTable('phone')">Asc</button>
      <button @click="sortDscTable('phone')">Dsc</button>
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
  sortedbyDesc.value = false;
}

function sortDscTable(sortBy) {
  sortedData.value.sort((a, b) => (a[sortBy] < b[sortBy] ? 1 : -1));
  sortedbyDesc.value = false;
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
