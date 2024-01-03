<template>
  <q-table
    class="q-mt-sm"
    row-key="name"
    :columns="columns"
    :rows="rows"
    selection="multiple"
    separator="cell"
    flat
    bordered
    >
    <template #body-cell-name="props">
      <q-td :props="props">
        <div>
          {{ props.row.name }}
          <q-tooltip>{{ props.row.name }}</q-tooltip>
        </div>
       </q-td>
    </template>
    <template #body-cell-cellphone="props">
      <q-td :props="props">
        <div>
          {{ props.row.cellphone }}
          <q-tooltip>{{ props.row.cellphone }}</q-tooltip>
        </div>
       </q-td>
    </template>
    <template #body-cell-email="props">
      <q-td :props="props">
        <div>
          {{ props.row.email }}
          <q-tooltip>{{ props.row.email }}</q-tooltip>
        </div>
       </q-td>
    </template>
    <template #body-cell-gender="props">
      <q-td :props="props">
        <div>
          {{ props.row.gender }}
          <q-tooltip>{{ props.row.gender }}</q-tooltip>
        </div>
       </q-td>
    </template>
    <template #body-cell-birthday="props">
      <q-td :props="props">
        <div>
          {{ formatBirthday(props.row.birthday) }}
          <q-tooltip>{{ formatBirthday(props.row.birthday) }}</q-tooltip>
        </div>
       </q-td>
    </template>
  </q-table>
</template>
<script setup>
import dayjs from 'dayjs'

defineProps({
  columns: {
    type: Array,
    default: () => [],
  },
  rows: {
    type: Array,
    default: () => [],
  }
})

const formatBirthday = (birthday) => {
  if(dayjs(birthday).isValid()){
    return dayjs(birthday).format('YYYY/MM/DD')
  }

  return ''
}

</script>
