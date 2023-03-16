<script setup lang='ts'>
import { onMounted, ref } from 'vue'
import { NSpin } from 'naive-ui'
import { fetchChatConfig } from '@/api'

interface ConfigState {
  timeoutMs?: number
  reverseProxy?: string
  apiModel?: string
  socksProxy?: string
  balance?: string
}

const loading = ref(false)
const config = ref<ConfigState>()

async function fetchConfigData() {
  try {
    loading.value = true
    const { data } = await fetchChatConfig<ConfigState>()
    config.value = data
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchConfigData()
})
</script>

<template>
  <NSpin :show="loading">
    <div class="p-4 space-y-4 " >
      <h2 class="text-xl font-bold">
        Version - 999
      </h2>
      <p>{{ $t("setting.api") }}：{{ config?.apiModel ?? '-' }}</p>
      <p>
        余额：{{ config?.balance ?? '0' }}$

      </p>
    </div>
  </NSpin>
</template>
