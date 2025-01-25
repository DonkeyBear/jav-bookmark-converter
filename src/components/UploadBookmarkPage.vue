<script setup>
import { NButton, NUpload } from 'naive-ui'

const modelValue = defineModel()
</script>

<template>
  <NUpload
    v-model:file-list="modelValue"
    :max="1"
    :style="{ display: 'flex', gap: '1rem', alignItems: 'center' }"
    :file-list-style="{ flex: 1, margin: '-6px auto' }"
  >
    <NButton>上傳</NButton>
  </NUpload>
</template>
