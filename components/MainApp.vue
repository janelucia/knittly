<template>
  <div class="min-h-screen bg-base-200/30 p-4 lg:p-6">
    <div class="max-w-5xl mx-auto space-y-4 lg:space-y-6">
      
      <!-- Compact Header -->
      <div class="card bg-base-100 shadow-sm border border-base-200">
        <div class="card-body p-4 lg:p-5">
          <div class="flex flex-col lg:flex-row lg:items-center lg:justify-between gap-4">
            <div class="text-center lg:text-left">
              <h1 class="text-2xl lg:text-3xl font-bold bg-gradient-to-r from-primary to-secondary bg-clip-text text-transparent">
                Sophie Scarf
              </h1>
              <p class="text-sm text-base-content/60">Strick-Assistent</p>
            </div>
            <div class="stats stats-horizontal bg-base-200/50 rounded-lg shadow-sm">
              <div class="stat place-items-center py-3 px-4">
                <div class="stat-title text-xs">Phase</div>
                <div class="stat-value text-primary text-base lg:text-lg">
                  {{
                    data.increase === data.increasesDone &&
                    data.decreasesDone >= data.decrease
                      ? "Finale Reihen"
                      : data.increase == data.increasesDone
                      ? "Abnahmen"
                      : "Zunahmen"
                  }}
                </div>
              </div>
              <div class="stat place-items-center py-3 px-4">
                <div class="stat-title text-xs">Fortschritt</div>
                <div class="stat-value text-secondary text-base lg:text-lg">
                  {{
                    data.increase == data.increasesDone
                      ? data.decreasesDone + 1
                      : data.increasesDone + 1
                  }}/{{
                    data.increase == data.increasesDone
                      ? data.decrease
                      : data.increase
                  }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Desktop Grid Layout -->
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-4 lg:gap-6">
        
        <!-- Main Counter (spans 2 columns on desktop) -->
        <div class="lg:col-span-2 space-y-4">
          <!-- Alert Messages -->
          <div
            v-if="
              data.rowsWorked == 7 &&
              data.increase === data.increasesDone &&
              data.decreasesDone >= data.decrease
            "
            class="alert alert-success shadow-sm"
          >
            <Icon name="material-symbols:celebration" class="text-xl" />
            <span><strong>Jetzt musst du abketten!</strong></span>
          </div>
          <div
            v-else-if="
              data.rowsWorked == 7 &&
              data.increase === data.increasesDone &&
              data.decreasesDone === data.decrease
            "
            class="alert alert-info shadow-sm"
          >
            <Icon name="material-symbols:info" class="text-xl" />
            <span><strong>Jetzt die letzten 7 Reihen stricken!</strong></span>
          </div>
          <div v-else-if="data.rowsWorked == 7" class="alert alert-warning shadow-sm">
            <Icon name="material-symbols:warning" class="text-xl" />
            <span>Zeit für eine <strong>{{
              data.increase === data.increasesDone ? "Abnahme" : "Zunahme"
            }}</strong>!</span>
          </div>

          <!-- Row Counter -->
          <div class="card bg-base-100 shadow-sm border border-base-200">
            <div class="card-body p-4 lg:p-6">
              <h2 class="text-lg lg:text-xl font-semibold text-center mb-4">Reihenzähler</h2>
              <div class="flex items-center justify-center gap-6">
                <button
                  @click="decreaseAlreadyKnittedRows"
                  class="btn btn-circle btn-lg btn-secondary shadow-md hover:shadow-lg transition-all duration-200"
                >
                  <Icon name="material-symbols:remove" class="text-xl" />
                </button>

                <div class="w-24 h-24 lg:w-28 lg:h-28 rounded-2xl bg-gradient-to-br from-primary to-secondary flex items-center justify-center shadow-lg">
                  <span class="text-3xl lg:text-4xl font-bold text-white">{{ data.rowsWorked }}</span>
                </div>

                <button
                  @click="data.rowsWorked >= 7 ? markBlockComplete() : increaseAlreadyKnittedRows()"
                  class="btn btn-circle btn-lg shadow-md hover:shadow-lg transition-all duration-200"
                  :class="data.rowsWorked >= 7 ? 'btn-success' : 'btn-primary'"
                >
                  <Icon 
                    :name="data.rowsWorked >= 7 ? 'material-symbols:check' : 'material-symbols:add'" 
                    class="text-xl" 
                  />
                </button>
              </div>
              
              <!-- Block completion indicator -->
              <div v-if="data.rowsWorked >= 7" class="text-center mt-3">
                <div class="text-sm text-success font-medium">Block bereit zum Abschließen!</div>
              </div>
            </div>
          </div>
        </div>

        <!-- Sidebar -->
        <div class="space-y-4">
          <!-- Progress Circle -->
          <div class="card bg-base-100 shadow-sm border border-base-200">
            <div class="card-body p-4 text-center">
              <h3 class="text-base font-semibold mb-3">Fortschritt</h3>
              <div class="flex justify-center">
                <div
                  class="radial-progress text-primary"
                  :style="{
                    '--value': progress,
                    '--size': '6rem',
                    '--thickness': '0.5rem',
                  }"
                >
                  <div class="text-center">
                    <div class="text-lg font-bold">{{ Math.round(progress) }}%</div>
                    <div class="text-xs opacity-70">Fertig</div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Project Stats -->
          <div class="card bg-base-100 shadow-sm border border-base-200">
            <div class="card-body p-4">
              <h3 class="text-base font-semibold mb-3">Projekt Details</h3>
              <div class="space-y-2 text-sm">
                <div class="flex justify-between">
                  <span class="text-base-content/70">Zunahmen:</span>
                  <span class="font-medium">{{ data.increasesDone }}/{{ data.increase }}</span>
                </div>
                <div class="flex justify-between">
                  <span class="text-base-content/70">Abnahmen:</span>
                  <span class="font-medium">{{ data.decreasesDone }}/{{ data.decrease }}</span>
                </div>
              </div>
            </div>
          </div>

          <!-- Tips Card -->
          <div class="card bg-gradient-to-br from-primary/5 to-secondary/5 border border-primary/20 shadow-sm">
            <div class="card-body p-4">
              <h3 class="text-base font-semibold mb-2 flex items-center gap-2">
                <Icon name="material-symbols:lightbulb" class="text-primary" />
                Tipp
              </h3>
              <p class="text-sm text-base-content/80 leading-relaxed">
                {{ currentTip }}
              </p>
            </div>
          </div>

          <!-- Help Video (Compact) -->
          <div class="card bg-base-100 shadow-sm border border-base-200">
            <div class="card-body p-4">
              <h3 class="text-base font-semibold mb-3">Video-Hilfe</h3>
              <div class="aspect-video rounded-lg overflow-hidden shadow-sm">
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
        </div>
      </div>

      <!-- Bottom Actions -->
      <div class="card bg-base-100 shadow-sm border border-base-200">
        <div class="card-body p-4">
          <div class="flex justify-center">
            <label for="reset-modal" class="btn btn-outline btn-error btn-sm">
              <Icon name="material-symbols:refresh" />
              Projekt zurücksetzen
            </label>
          </div>

          <!-- Reset Modal -->
          <input type="checkbox" id="reset-modal" class="modal-toggle" />
          <div class="modal">
            <div class="modal-box">
              <h3 class="font-bold text-lg text-error mb-4">Projekt zurücksetzen?</h3>
              <p class="mb-6">Alle Daten gehen dauerhaft verloren.</p>
              <label class="cursor-pointer label justify-start gap-3 mb-6">
                <input
                  type="checkbox"
                  v-model="confirmReset"
                  class="checkbox checkbox-error"
                />
                <span class="label-text">Ich bin mir sicher</span>
              </label>
              <div class="modal-action">
                <label for="reset-modal" class="btn">Abbrechen</label>
                <label
                  for="reset-modal"
                  class="btn btn-error"
                  :class="{ 'btn-disabled': !confirmReset }"
                  @click="confirmReset ? handleReset() : null"
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
interface KnittingData {
  rowsWorked: number
  increasesDone: number
  decreasesDone: number
  increase: number
  decrease: number
}

const props = defineProps<{
  data: KnittingData
}>()

const emit = defineEmits<{
  updateData: [data: KnittingData]
  reset: []
  done: []
}>()

const confirmReset = ref<boolean>(false)

const progress = computed(() => {
  const totalSteps = (props.data.increase + props.data.decrease) * 8 + 7
  let completedSteps = (props.data.increasesDone + props.data.decreasesDone) * 8

  if (props.data.decrease > 0 && props.data.decreasesDone >= props.data.decrease) {
    completedSteps += props.data.rowsWorked
  } else {
    completedSteps += props.data.rowsWorked
  }

  return totalSteps > 0 ? (completedSteps / totalSteps) * 100 : 0
})


const tips = [
  "Zähle deine Maschen regelmäßig, um Fehler früh zu erkennen.",
  "Verwende Maschenmarkierer für bessere Orientierung.",
  "Mache alle 10 Reihen eine kurze Pause für deine Hände.",
  "Dokumentiere schwierige Stellen für künftige Projekte.",
  "Bei Fehlern: Nicht gleich auftrennen, manchmal lässt sich korrigieren.",
]

const currentTip = computed(() => {
  const tipIndex = (props.data.increasesDone + props.data.decreasesDone) % tips.length
  return tips[tipIndex]
})

const increaseAlreadyKnittedRows = () => {
  const newData = { ...props.data }
  
  if (newData.rowsWorked >= 7) {
    newData.rowsWorked = 0
    if (
      newData.increase === newData.increasesDone &&
      newData.decreasesDone >= newData.decrease
    ) {
      return
    } else if (newData.increase == newData.increasesDone) {
      newData.decreasesDone++
    } else {
      newData.increasesDone++
    }
  } else {
    newData.rowsWorked++
  }
  
  emit('updateData', newData)
  checkCompletion(newData)
}

const decreaseAlreadyKnittedRows = () => {
  if (props.data.rowsWorked > 0) {
    const newData = { ...props.data }
    newData.rowsWorked--
    emit('updateData', newData)
  }
}

const markBlockComplete = () => {
  if (props.data.rowsWorked >= 7) {
    increaseAlreadyKnittedRows()
  }
}


const checkCompletion = (data: KnittingData) => {
  if (
    data.increase === data.increasesDone &&
    data.decreasesDone >= data.decrease &&
    data.rowsWorked >= 7
  ) {
    emit('done')
  }
}

const handleReset = () => {
  confirmReset.value = false
  emit('reset')
}
</script>