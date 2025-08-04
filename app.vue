<template>
  <div class="min-h-screen bg-gradient-to-br from-base-300 via-base-100 to-base-200">
    <!-- Setup Screen -->
    <div v-if="!informationSaved" class="min-h-screen flex items-center justify-center px-6">
      <div class="card w-full max-w-md bg-base-100 shadow-2xl">
        <div class="card-body space-y-6">
          <div class="text-center space-y-4">
            <div class="w-16 h-16 mx-auto bg-gradient-to-r from-primary to-secondary rounded-2xl flex items-center justify-center">
              <Icon name="material-symbols:content-cut" class="text-2xl text-white" />
            </div>
            <h1 class="text-3xl font-bold bg-gradient-to-r from-primary to-secondary bg-clip-text text-transparent">
              Knittly
            </h1>
            <p class="text-base-content/70">
              Dein persönlicher Strick-Assistent für den Sophie Scarf
            </p>
          </div>

          <div class="space-y-4">
            <div class="form-control">
              <label class="label">
                <span class="label-text font-semibold">Anzahl Zunahmen</span>
              </label>
              <input 
                v-model="data.increase" 
                type="number" 
                min="0" 
                step="1" 
                class="input input-bordered input-lg text-center text-xl font-bold" 
                placeholder="0"
              />
            </div>

            <div v-if="data.increase > 0" class="form-control">
              <label class="label">
                <span class="label-text font-semibold">Bereits gemachte Zunahmen</span>
              </label>
              <input 
                v-model="data.increasesDone" 
                type="number" 
                min="0" 
                step="1" 
                :max="data.increase"
                class="input input-bordered input-lg text-center text-xl font-bold" 
                placeholder="0"
              />
            </div>

            <div v-if="data.increase > 0 && data.increasesDone == data.increase" class="form-control">
              <label class="label">
                <span class="label-text font-semibold">Bereits gemachte Abnahmen</span>
              </label>
              <input 
                v-model="data.decreasesDone" 
                type="number" 
                min="0" 
                step="1" 
                :max="data.decrease"
                class="input input-bordered input-lg text-center text-xl font-bold" 
                placeholder="0"
              />
            </div>

            <div class="form-control">
              <label class="label">
                <span class="label-text font-semibold">Reihen im aktuellen Block</span>
              </label>
              <input 
                v-model="data.rowsWorked" 
                type="number" 
                min="0" 
                step="1" 
                max="7"
                class="input input-bordered input-lg text-center text-xl font-bold" 
                placeholder="0"
              />
            </div>

            <button 
              v-if="data.increase > 0"
              @click="startApp" 
              class="btn btn-primary btn-lg w-full text-white font-semibold shadow-lg hover:shadow-xl transform hover:scale-105 transition-all duration-200"
            >
              <Icon name="material-symbols:play-arrow" class="text-xl" />
              Los geht's!
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Completion Screen -->
    <div v-else-if="done" class="min-h-screen flex items-center justify-center px-6">
      <div class="card w-full max-w-md bg-base-100 shadow-2xl">
        <div class="card-body text-center space-y-6">
          <div class="text-8xl">🎉</div>
          <h1 class="text-3xl font-bold bg-gradient-to-r from-primary to-secondary bg-clip-text text-transparent">
            Herzlichen Glückwunsch!
          </h1>
          <p class="text-lg text-base-content/70">
            Du hast deinen Sophie Scarf erfolgreich abgeschlossen!
          </p>
          <button @click="reset" class="btn btn-primary btn-lg w-full">
            <Icon name="material-symbols:refresh" class="text-xl" />
            Neues Projekt starten
          </button>
        </div>
      </div>
    </div>

    <!-- Main App Screen -->
    <div v-else class="min-h-screen p-6 space-y-6">
      <!-- Header -->
      <div class="card bg-base-100 shadow-lg">
        <div class="card-body text-center space-y-4">
          <h1 class="text-2xl font-bold">Sophie Scarf</h1>
          <div class="stats stats-horizontal shadow">
            <div class="stat">
              <div class="stat-title">Phase</div>
              <div class="stat-value text-primary text-lg">
                {{ data.increase == data.increasesDone ? 'Abnahmen' : 'Zunahmen' }}
              </div>
            </div>
            <div class="stat">
              <div class="stat-title">Fortschritt</div>
              <div class="stat-value text-secondary text-lg">{{ Math.round(progress) }}%</div>
            </div>
          </div>
        </div>
      </div>

      <!-- Row Counter -->
      <div class="card bg-base-100 shadow-lg">
        <div class="card-body space-y-6">
          <div v-if="data.rowsWorked == 7" class="alert alert-warning">
            <Icon name="material-symbols:warning" />
            <span>Zeit für eine <strong>{{ data.increase === data.increasesDone ? 'Abnahme' : 'Zunahme' }}</strong>!</span>
          </div>

          <div class="text-center space-y-4">
            <h2 class="text-xl font-semibold">Reihen-Zähler</h2>
            <div class="flex items-center justify-center gap-4">
              <button 
                @click="increaseAlreadyKnittedRows"
                class="btn btn-circle btn-lg btn-primary shadow-lg hover:shadow-xl transform hover:scale-110 transition-all"
              >
                <Icon name="material-symbols:add" class="text-2xl" />
              </button>
              
              <div class="w-24 h-24 rounded-3xl bg-gradient-to-r from-primary to-secondary flex items-center justify-center shadow-lg">
                <span class="text-4xl font-bold text-white">{{ data.rowsWorked }}</span>
              </div>
              
              <button 
                @click="decreaseAlreadyKnittedRows"
                class="btn btn-circle btn-lg btn-secondary shadow-lg hover:shadow-xl transform hover:scale-110 transition-all"
              >
                <Icon name="material-symbols:remove" class="text-2xl" />
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Progress Visualization -->
      <div class="card bg-base-100 shadow-lg">
        <div class="card-body">
          <h3 class="text-lg font-semibold mb-4">Projekt Übersicht</h3>
          <div class="flex justify-center">
            <div class="radial-progress text-primary" :style="{ '--value': progress, '--size': '10rem', '--thickness': '1rem' }">
              <div class="text-center">
                <div class="text-2xl font-bold">{{ Math.round(progress) }}%</div>
                <div class="text-sm opacity-70">Fertig</div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Help Video -->
      <div class="card bg-base-100 shadow-lg">
        <div class="card-body">
          <h3 class="text-lg font-semibold mb-4">Hilfe-Video</h3>
          <div class="aspect-video rounded-xl overflow-hidden">
            <iframe
              v-if="data.increase !== data.increasesDone"
              class="w-full h-full"
              src="https://www.youtube.com/embed/T26XaiSL0aM?mute=1&autoplay=0"
              title="Zunahmen Tutorial"
              allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            />
            <iframe
              v-else
              class="w-full h-full"
              src="https://www.youtube.com/embed/mxM5XXCLDlg?mute=1&autoplay=0"
              title="Abnahmen Tutorial"
              allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            />
          </div>
        </div>
      </div>

      <!-- Reset Button -->
      <div class="card bg-base-100 shadow-lg">
        <div class="card-body">
          <label for="reset-modal" class="btn btn-outline btn-error w-full">
            <Icon name="material-symbols:refresh" />
            Projekt zurücksetzen
          </label>

          <input type="checkbox" id="reset-modal" class="modal-toggle" />
          <div class="modal">
            <div class="modal-box">
              <h3 class="font-bold text-lg text-error mb-4">Projekt zurücksetzen?</h3>
              <p class="mb-6">Alle Daten gehen dauerhaft verloren.</p>
              
              <label class="cursor-pointer label justify-start gap-3 mb-6">
                <input type="checkbox" v-model="confirmReset" class="checkbox checkbox-error" />
                <span class="label-text">Ich bin mir sicher</span>
              </label>

              <div class="modal-action">
                <label for="reset-modal" class="btn">Abbrechen</label>
                <label 
                  for="reset-modal" 
                  class="btn btn-error"
                  :class="{ 'btn-disabled': !confirmReset }"
                  @click="confirmReset ? reset() : null"
                >
                  Zurücksetzen
                </label>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const data = reactive({
  rowsWorked: 0,
  increasesDone: 0,
  decreasesDone: 0,
  increase: 0,
  decrease: 0
})

const informationSaved = ref<boolean>(false)
const confirmReset = ref<boolean>(false)
const done = ref<boolean>(false)

const saveData = () => {
  localStorage.setItem('knittingData', JSON.stringify(data))
}

const startApp = () => {
  data.decrease = data.increase
  informationSaved.value = true
  saveData()
}

const loadData = () => {
  const savedData = localStorage.getItem('knittingData')
  if (savedData) {
    Object.assign(data, JSON.parse(savedData))
    informationSaved.value = true
  }
}

const progress = computed(() => {
  const totalSteps = (data.increase + data.decrease) * 8
  const completedSteps = (data.increasesDone + data.decreasesDone) * 8 + data.rowsWorked
  return totalSteps > 0 ? (completedSteps / totalSteps) * 100 : 0
})

const increaseAlreadyKnittedRows = () => {
  if (data.rowsWorked >= 7) {
    data.rowsWorked = 0
    if (data.increase == data.increasesDone) {
      data.decreasesDone++
    } else {
      data.increasesDone++
    }
  } else {
    data.rowsWorked++
  }
  saveData()
}

const decreaseAlreadyKnittedRows = () => {
  if (data.rowsWorked > 0) {
    data.rowsWorked--
    saveData()
  }
}

const reset = () => {
  localStorage.removeItem('knittingData')
  Object.assign(data, {
    rowsWorked: 0,
    increasesDone: 0,
    decreasesDone: 0,
    increase: 0,
    decrease: 0
  })
  informationSaved.value = false
  confirmReset.value = false
  done.value = false
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
    done.value = informationSaved.value && data.increase === data.increasesDone && newValue >= data.decrease
  }
)

watch(
  () => data.rowsWorked,
  () => {
    if (informationSaved.value) saveData()
  }
)

onMounted(() => {
  loadData()
})
</script>