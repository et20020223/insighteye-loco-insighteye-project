<template>
  <q-dialog v-model="isOpen">
    <q-card class="my-card" style="width: 400px">
      <q-card-section class="row items-center q-pb-none">
        <q-space />
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>
      <q-card-section class="q-pt-none text-center">
        <div class="text-h5 q-mb-xl">
          新增
        </div>
        <q-form class="q-gutter-md">
          <q-input
            v-model="form.name"
            class="q-mb-sm"
            color="primary"
            outlined
            label="姓名"
            :rules="[ val => val && val.length > 0 || '請輸入姓名']">
          </q-input>
          <q-input
            v-model="form.cellphone" 
            class="q-mb-sm"
            color="primary" 
            outlined 
            label="手機"
            :rules="[ val => val && val.length > 0 || '請輸入手機']">
          </q-input>
          <q-input
            v-model="form.email"
            class="q-mb-sm"
            color="primary"
            outlined
            label="信箱"
            :rules="[ val => val && validEmail(val) || '請輸入信箱']">
          </q-input>
          <q-input
            v-model="form.birthday"
            outlined
            label="生日"
            mask="date"
            :rules="['date']">
            <template #append>
              <q-icon name="event" class="cursor-pointer">
                <q-popup-proxy cover transition-show="scale" transition-hide="scale">
                  <q-date v-model="form.birthday">
                    <div class="row items-center justify-end">
                      <q-btn v-close-popup label="Close" color="primary" flat />
                    </div>
                  </q-date>
                </q-popup-proxy>
              </q-icon>
            </template>
          </q-input>
          <div class="row justify-center items-center q-mb-sm">
            <span>性別：</span>
            <q-radio v-model="form.gender" val="男" label="男" />
            <q-radio v-model="form.gender" val="女" label="女" />
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
        </q-form>
      </q-card-section>
    </q-card>
  </q-dialog>
</template>
<script setup>
import { computed, reactive } from 'vue'

const props = defineProps({
  modelValue: {
    required: true,
    type: Boolean
  }
})

const emit = defineEmits({
    'update:modelValue': (_value) => true,
    send: (_item) => true
})

const form = reactive({
  name: '',
  cellphone: '',
  email: '',
  gender: '',
  birthday: ''
})

const isOpen = computed({
  get: () => props.modelValue,
  set: value => emit('update:modelValue', value)
})

const confirm = () => {
  emit('send', form)
  emit('update:modelValue', false)
  clearState()
}

const cancel = () => {
  emit('update:modelValue', false)
  clearState()
}

const clearState = () => {
  form.name = ''
  form.cellphone= ''
  form.email = ''
  form.gender = ''
  form.birthday = ''
}

const validEmail = (email) => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

  if (emailRegex.test(email)) {
    return true
  } 
  return false
}

</script>
