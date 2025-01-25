<script setup>
import { saveAs } from 'file-saver'
import {
  NButton,
  NCard,
  NFlex,
  NGrid,
  NGridItem,
  NH1,
  NStep,
  NSteps,
  useMessage,
} from 'naive-ui'
import { ref } from 'vue'
import { rulesDict } from '../utils/rulesDict'
import SelectBrowserPage from './SelectBrowserPage.vue'
import SettingRulesPage from './SettingRulesPage.vue'
import UploadBookmarkPage from './UploadBookmarkPage.vue'

const message = useMessage()

const currentStep = ref(1)

const inputs = ref({
  browser: null,
  files: [],
  rules: [],
})

async function convert() {
  try {
    const { file, name } = inputs.value.files[0]
    let fileContent = await file.text()
    const { rules } = inputs.value

    rules.forEach(({ from, to }) => {
      if (from && to) {
        const { regex } = rulesDict?.[from]
        fileContent = fileContent.replaceAll(regex, to)
      }
    })

    const blob = new Blob([fileContent], { type: 'text/html;charset=utf-8' })
    saveAs(blob, `new_${name}`)
    message.success('轉換成功！')
  }
  catch (error) {
    console.error(error)
    message.error('轉換失敗！\n請檢查檔案格式或規則設置。')
  }
}
</script>

<template>
  <NFlex justify="center" align="center" :style="{ maxheight: '100vh' }">
    <NCard :style="{ maxWidth: 'min(100vw, 50rem)', margin: '1rem' }">
      <template #header>
        <NH1 style="margin-bottom: 0.5rem">
          JAV 書籤轉換器
        </NH1>
      </template>
      <NGrid :cols="3" style="gap: 1rem">
        <NGridItem>
          <NSteps v-model:current="currentStep" vertical>
            <NStep title="瀏覽器" description="選擇你所使用的瀏覽器。" />
            <NStep title="書籤" description="上傳瀏覽器匯出的書籤（我的最愛）檔案。" />
            <NStep title="轉換規則" description="設置你所需要的轉換規則。" />
          </NSteps>
        </NGridItem>
        <NGridItem
          :span="2" :style="{
            borderLeft: '1px solid var(--n-border-color)',
            paddingLeft: '1rem',
          }"
        >
          <SelectBrowserPage
            v-if="currentStep === 1"
            v-model="inputs.browser"
          />
          <UploadBookmarkPage
            v-if="currentStep === 2"
            v-model="inputs.files"
          />
          <SettingRulesPage
            v-if="currentStep === 3"
            v-model="inputs.rules"
          />
        </NGridItem>
      </NGrid>
      <template #action>
        <NFlex justify="space-between">
          <NButton
            secondary
            :disabled="currentStep === 1"
            @click="currentStep--"
          >
            上一步
          </NButton>
          <NButton
            v-if="currentStep !== 3"
            type="primary"
            secondary
            :disabled="currentStep === 3"
            @click="currentStep++"
          >
            下一步
          </NButton>
          <NButton
            v-else
            type="primary"
            @click="convert()"
          >
            開始轉換
          </NButton>
        </NFlex>
      </template>
    </NCard>
  </NFlex>
</template>
