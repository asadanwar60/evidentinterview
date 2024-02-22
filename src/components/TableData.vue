<template>
  <table>
    <tr>
      <th v-for="item of columnName" :key="item.index">
        <slot :name="`col-${item}`" :column="item">{{ item }}</slot>

        <button
          v-if="sorting"
          @click="$emit('sortAscTable', item.toLowerCase())"
        >
          Asc
        </button>
        <button
          v-if="sorting"
          @click="$emit('sortDscTable', item.toLowerCase())"
        >
          Dsc
        </button>

        <slot />
      </th>
    </tr>

    <tr v-for="rowData in sortedData" :key="rowData.slno">
      <td v-for="item in columnName" :key="item">
        <slot
          :name="`rows-${item}`"
          :rows="rowData"
          :index="rowData.index"
        > {{ rowData[item.toLowerCase()] }}</slot>
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
  sorting:Boolean
});
</script>
