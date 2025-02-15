<script setup lang="ts">
import type { OpenAPIV2, OpenAPIV3, OpenAPIV3_1 } from '@scalar/openapi-types'
import type { Spec } from '@scalar/types/legacy'
import GithubSlugger from 'github-slugger'
import { computed } from 'vue'

import DownloadLink from '../../../features/DownloadLink/DownloadLink.vue'
import { Badge } from '../../Badge'
import {
  Section,
  SectionColumn,
  SectionColumns,
  SectionContainer,
  SectionContent,
  SectionHeader,
} from '../../Section'
import Description from './Description.vue'

const props = defineProps<{
  info: Partial<
    OpenAPIV2.InfoObject | OpenAPIV3.InfoObject | OpenAPIV3_1.InfoObject
  >
  parsedSpec: Spec
}>()

const slugger = new GithubSlugger()

const specVersion = computed(() => {
  return props.parsedSpec.openapi ?? props.parsedSpec.swagger ?? ''
})

const formattedSpecTitle = computed(() => {
  return slugger.slug(props.info.title ?? '')
})
</script>
<template>
  <SectionContainer>
    <Section class="introduction-section">
      <SectionContent :loading="!info.description && !info.title">
        <div class="badges">
          <Badge v-if="info.version">
            {{ info.version }}
          </Badge>
          <Badge v-if="specVersion"> OAS {{ specVersion }}</Badge>
        </div>
        <SectionHeader
          :level="1"
          :loading="!info.title"
          tight>
          {{ info.title }}
        </SectionHeader>
        <DownloadLink :specTitle="formattedSpecTitle" />
        <SectionColumns>
          <SectionColumn>
            <Description :value="info.description" />
          </SectionColumn>
          <SectionColumn v-if="$slots.aside">
            <div class="sticky-cards">
              <slot name="aside" />
            </div>
          </SectionColumn>
        </SectionColumns>
      </SectionContent>
      <slot name="after" />
    </Section>
  </SectionContainer>
</template>
<style scoped>
.heading {
  margin-top: 0px !important;
  word-wrap: break-word;
}
.loading {
  background: var(--scalar-background-3);
  animation: loading-skeleton 1.5s infinite alternate;
  border-radius: var(--scalar-radius-lg);
}
.badges {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-bottom: 3px;
}
.heading.loading {
  width: 80%;
}
.introduction-section {
  gap: 48px;
}
.sticky-cards {
  display: flex;
  flex-direction: column;

  position: sticky;
  top: calc(var(--refs-header-height) + 24px);
}
</style>
