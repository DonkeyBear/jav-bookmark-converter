<script setup>
import { NSelect } from 'naive-ui'

const modelValue = defineModel()
const options = [
  {
    label: 'Microsoft Edge',
    value: 'edge',
  },
  {
    label: 'Google Chrome',
    value: 'chrome',
  },
  {
    label: 'Mozilla Firefox',
    value: 'firefox',
  },
]
</script>

<template>
  <NSelect v-model:value="modelValue" :options="options" />
</template>
