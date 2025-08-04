<template>
  <div class="min-h-screen bg-gradient-to-br from-base-300 via-base-100 to-base-200">
    <!-- Setup Screen -->
    <Setup 
      v-if="!informationSaved" 
      @start-app="handleStartApp"
    />

    <!-- Completion Screen -->
    <Completion 
      v-else-if="done" 
      @reset="handleReset"
    />

    <!-- Main App Screen -->
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
interface KnittingData {
  rowsWorked: number
  increasesDone: number
  decreasesDone: number
  increase: number
  decrease: number
}

const data = reactive<KnittingData>({
  rowsWorked: 0,
  increasesDone: 0,
  decreasesDone: 0,
  increase: 0,
  decrease: 0
})

const informationSaved = ref<boolean>(false)
const done = ref<boolean>(false)

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
  Object.assign(data, {
    rowsWorked: 0,
    increasesDone: 0,
    decreasesDone: 0,
    increase: 0,
    decrease: 0
  })
  informationSaved.value = false
  done.value = false
}

const handleDone = () => {
  done.value = true
}

// Auto-save watchers
watch(
  () => data.increase,
  () => {
    if (informationSaved.value) saveData()
  }
)

watch(
  () => data.increasesDone,
  () => {
    if (informationSaved.value) saveData()
  }
)

watch(
  () => data.decreasesDone,
  (newValue) => {
    if (informationSaved.value) saveData()
    done.value =
      informationSaved.value &&
      data.increase === data.increasesDone &&
      newValue >= data.decrease &&
      data.rowsWorked >= 7
  }
)

watch(
  () => data.rowsWorked,
  () => {
    if (informationSaved.value) saveData()
    // Check if we're complete (all increases, decreases done + final 7 rows)
    if (
      data.increase === data.increasesDone &&
      data.decreasesDone >= data.decrease &&
      data.rowsWorked >= 7
    ) {
      done.value = true
    }
  }
)

onMounted(() => {
  loadData()
})
</script>