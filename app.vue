<template>
  <div class="min-h-screen bg-gradient-to-br from-base-300 via-base-100 to-base-200">
    <Setup
      v-if="!informationSaved"
      @start-app="handleStartApp"
    />
    <Completion
      v-else-if="done"
      @reset="handleReset"
    />
    <MainApp
      v-else 
      :data="data"
      @update-data="handleUpdateData"
      @reset="handleReset"
      @done="handleDone"
    />
  </div>
</template>

<script setup lang="ts">
import type { KnittingData } from '~/types/knitting'

const defaultData = (): KnittingData => ({
  rowsWorked: 0,
  increasesDone: 0,
  decreasesDone: 0,
  increase: 0,
  decrease: 0,
})

const data = reactive<KnittingData>(defaultData())
const informationSaved = ref(false)
const done = ref(false)

const saveData = () => {
  localStorage.setItem('knittingData', JSON.stringify(data))
}

const loadData = () => {
  const savedData = localStorage.getItem('knittingData')
  if (savedData) {
    Object.assign(data, JSON.parse(savedData))
    informationSaved.value = true
  }
}

const handleStartApp = (setupData: KnittingData) => {
  Object.assign(data, setupData)
  informationSaved.value = true
  saveData()
}

const handleUpdateData = (newData: KnittingData) => {
  Object.assign(data, newData)
  saveData()
}

const handleReset = () => {
  localStorage.removeItem('knittingData')
  Object.assign(data, defaultData())
  informationSaved.value = false
  done.value = false
}

const handleDone = () => {
  done.value = true
}

watch(data, () => {
  if (informationSaved.value) saveData()
}, { deep: true })

onMounted(() => {
  loadData()
})
</script>