<template>
  <table>
    <tr>
      <th v-for="item of columnName" :key="item.index">
        <slot :name="`col-${item}`" :column="item"></slot>
      </th>
    </tr>

    <tr v-for="rowData in sortedData" :key="rowData.slno">
      <td v-for="item in columnName" :key="item">
        <slot
          :name="`rows-${item}`"
          :rows="rowData"
          :index="rowData.index"
        ></slot>
      </td>
    </tr>
  </table>
</template>

<script setup>
defineProps({
  sortedData: {
    type: Array,
    required: true,
  },
  columnName: {
    type: Array,
    required: true,
  },
});
</script>
