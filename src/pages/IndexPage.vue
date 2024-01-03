<template>
  <q-page class="row justify-left q-pa-lg">
    <div class="block-item">
      <div class="row justify-between items-center">
        <div class="text-h6">員工基本資訊</div>
        <div>
          <q-btn
            class="q-mr-sm"
            color="primary"
            label="新增"
            @click="openDialog('add')" />
          <q-btn
            color="white"
            text-color="black"
            label="刪除"
            @click="openDialog('delete')"/>
        </div>
      </div>
      <DataFilters @send="syncRowData" @loading="syncLoading"></DataFilters>
      <QTable
        v-model:selected="state.selected"
        class="q-mt-md"
        :loading="state.loading"
        :columns="state.columns"
        :rows="state.rows">
      </QTable>
    </div>
    <AddDialog
      v-model="state.addDialog"
      @send="add">
    </AddDialog>
    <DeleteDialog
      v-model="state.deleteDialog"
      @remove="remove"
      :number="state.selected.length">
    </DeleteDialog>
  </q-page>
</template>

<script setup>
import QTable from 'src/components/QTable.vue'
import DataFilters from 'src/components/DataFilters.vue'
import AddDialog from 'src/components/AddDialog.vue'
import DeleteDialog from 'src/components/DeleteDialog.vue'
import { reactive, onMounted } from 'vue'
import { useQuasar } from 'quasar'
import axios from 'axios'

const $q = useQuasar()

const state = reactive({
  columns: [
    {
      name: 'name',
      label: '姓名',
      align: 'left',
      field: 'name',
      sortable: true
    },
    {
      name: 'cellphone',
      label: '手機',
      align: 'left',
      field: 'cellphone',
      sortable: true
    },
    {
      name: 'email',
      label: '信箱',
      align: 'left',
      field: 'email',
      sortable: true
    },
    {
      name: 'gender',
      label: '性別',
      align: 'left',
      field: 'gender',
      sortable: true
    },
    {
      name: 'birthday',
      label: '生日',
      align: 'left',
      field: 'birthday',
      sortable: true
    },
  ],
  rows: [],
  selected: [],
  addDialog: false,
  deleteDialog: false,
  loading: false
})

onMounted(()=>{
  getMembers()
})

const remove = () => {
  state.rows = state.rows.filter(item => !state.selected.includes(item))
  state.selected = []
  $q.notify({
    type: 'positive',
    message: '刪除成功'
  })
}

const add = (item) => {
  const data = Object.assign({}, item)
  state.rows = [data, ...state.rows]
  $q.notify({
    type: 'positive',
    message: '新增成功'
  })
}

const openDialog = (type) => {
  if(state.selected.length === 0 && type === 'delete') {
    $q.notify({
      type: 'warning',
      message: '請選擇要刪除的資料'
    })
    return false
  }

  if(type === 'delete') {
    state.deleteDialog = true
  } else {
    state.addDialog = true
  }
}

const getMembers = async() => {
  state.loading = true
  await axios.get('http://35.194.177.50:7777/members')
    .then(response => {
      state.rows = response.data.members
    })
    .catch(error => {
      $q.notify({
        type: 'negative',
        message: error
      })
    })
  state.loading = false
}

const syncRowData = (data) => {
  state.rows = data
}

const syncLoading = (loading) => {
  state.loading = loading
}

</script>
<style lang="scss" scoped>
.block-item {
  min-width: 400px;
}
</style>
