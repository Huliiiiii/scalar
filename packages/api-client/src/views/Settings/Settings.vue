<script setup lang="ts">
import ViewLayout from '@/components/ViewLayout/ViewLayout.vue'
import ViewLayoutContent from '@/components/ViewLayout/ViewLayoutContent.vue'
import { type DefineComponent, ref } from 'vue'

import SettingsGeneral from './SettingsGeneral.vue'

type SettingsGroup = {
  component: DefineComponent<any, any, any>
  title: string
}

const settings: Record<string, SettingsGroup> = {
  general: {
    component: SettingsGeneral,
    title: 'general',
  },
}

const activeSetting = ref('general')
</script>
<template>
  <ViewLayout>
    <ViewLayoutContent class="flex-1">
      <component :is="settings[activeSetting].component" />
    </ViewLayoutContent>
  </ViewLayout>
</template>
