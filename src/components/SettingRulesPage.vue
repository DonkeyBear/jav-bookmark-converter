<script setup>
import { NDynamicInput, NFlex, NSelect, NText } from 'naive-ui'
import { rulesDict } from '../utils/rulesDict'

const modelValue = defineModel()

function onCreate() {
  return {
    from: null,
    to: null,
  }
}
const optionsFrom = Object.keys(rulesDict).map(key => ({
  label: key,
  value: key,
}))

function getOptionsTo(from) {
  if (rulesDict[from]) {
    return rulesDict[from].options.map(option => ({
      label: option,
      value: option,
    }))
  }
  else {
    return []
  }
}

function getIsDisabled(from) {
  return !rulesDict[from]
}
</script>

<template>
  <NDynamicInput v-model:value="modelValue" @create="onCreate">
    <template #default="{ value }">
      <NFlex style="width: 100%;" :wrap="false" align="center">
        <NSelect
          v-model:value="value.from"
          :options="optionsFrom"
        />
        <NText :depth="3">
          →
        </NText>
        <NSelect
          v-model:value="value.to"
          :options="getOptionsTo(value.from)"
          :disabled="getIsDisabled(value.from)"
        />
      </NFlex>
    </template>
  </NDynamicInput>
</template>
