<script setup>
import {
  darkTheme,
  NConfigProvider,
  NGlobalStyle,
  NMessageProvider,
  zhTW,
} from 'naive-ui'
import AppContent from './components/AppContent.vue'
</script>

<template>
  <NConfigProvider :theme="darkTheme" :locale="zhTW">
    <NMessageProvider>
      <AppContent />
    </NMessageProvider>
    <NGlobalStyle />
  </NConfigProvider>
</template>
