<template>
  <q-dialog v-model="isOpen">
      <q-card class="my-card" style="width: 300px">
        <q-card-section class="row items-center q-pb-none">
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>
        <q-card-section class="q-pt-none text-center">
          <div class="text-h5">
            刪除
          </div>
          <div class="text-subtitle1 q-mt-md">
            是否確定刪除 {{ number }} 筆資料 ?
          </div>
          <div class="q-mt-lg">
            <q-btn
              class="q-mr-md"
              color="white"
              text-color="black"
              label="取消"
              @click="cancel"/>
            <q-btn
              color="primary"
              label="確定"
              @click="confirm" />
          </div>
        </q-card-section>
      </q-card>
    </q-dialog>
</template>
<script setup>
import { computed } from 'vue'

const props = defineProps({
  modelValue: {
    required: true,
    type: Boolean
  },
  number: {
    type: Number,
    default: 0,
  }
})

const emit = defineEmits({
  'update:modelValue': (_value) => true,
  remove: () => true
})

const isOpen = computed({
  get: () => props.modelValue,
  set: value => emit('update:modelValue', value)
})

const confirm = () => {
  emit('remove')
  emit('update:modelValue', false)
}

const cancel = () => {
  emit('update:modelValue', false)
}

</script>
