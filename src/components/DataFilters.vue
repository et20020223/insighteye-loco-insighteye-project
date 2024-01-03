<template>
  <div class="row q-my-sm">
    <div class="col">
      <q-input
        v-model="state.filter.name"
        debounce="500"
        class="q-mr-sm"
        color="primary"
        outlined
        label="搜尋姓名">
        <template v-if="state.filter.name" #append>
          <q-icon
            name="cancel"
            class="cursor-pointer"
            @click.stop.prevent="state.filter.name = ''" />
        </template>
      </q-input>
    </div>
    <div class="col">
      <q-input
        v-model="state.filter.cellphone"
        debounce="500"
        color="primary"
        outlined
        label="搜尋手機">
        <template v-if="state.filter.cellphone" #append>
          <q-icon
            name="cancel"
            class="cursor-pointer"
            @click.stop.prevent="state.filter.cellphone = ''" />
        </template>
      </q-input>
    </div>
  </div>
  <div class="row q-my-sm">
    <div class="col">
      <q-input
        v-model="state.filter.email"
        debounce="500"
        class="q-mr-sm"
        color="primary"
        outlined
        label="搜尋信箱">
        <template v-if="state.filter.email" #append>
          <q-icon
            name="cancel"
           class="cursor-pointer"
           @click.stop.prevent="state.filter.email = ''" />
        </template>
      </q-input>
    </div>
    <div class="col">
      <q-select
        v-model="state.filter.gender"
        :options="state.options"
        outlined
        label="搜尋性別">
        <template v-if="state.filter.gender" #append>
          <q-icon
            name="cancel"
            class="cursor-pointer"
            @click.stop.prevent="state.filter.gender = ''" />
        </template>
      </q-select>
    </div>
  </div>
</template>
<script setup>
import { watch, reactive } from 'vue'
import { useQuasar } from 'quasar'
import axios from 'axios'

const $q = useQuasar()

const emit = defineEmits({
  loading: (_value) => true, 
  send: (_data) => true
})

const state = reactive({
  options: ['男', '女'],
  filter: {
    name: '',
    cellphone: '',
    email: '',
    gender: ''
  }
})

watch(() => state.filter, () => search(), {
  deep: true
})

const search = async() => {
  emit('loading', true)
  await axios.post('http://35.194.177.50:7777/members/search', state)
  .then(response => {
    emit('send', response.data.members)
  })
  .catch(error => {
    $q.notify({
      type: 'negative',
      message: error
    })
  })
  emit('loading', false)
}

</script>
