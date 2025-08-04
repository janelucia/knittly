<template>
  <div class="min-h-screen p-6 space-y-6">
    <!-- Header -->
    <div class="card bg-base-100 shadow-lg">
      <div class="card-body text-center space-y-4">
        <h1 class="text-2xl font-bold">Sophie Scarf</h1>
        <div class="stats stats-horizontal shadow">
          <div class="stat">
            <div class="stat-title">Phase</div>
            <div class="stat-value text-primary text-lg">
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
          <div class="stat">
            <div class="stat-title">Aktuell</div>
            <div class="stat-value text-secondary text-lg">
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

    <!-- Row Counter -->
    <div class="card bg-base-100 shadow-lg">
      <div class="card-body space-y-6">
        <div
          v-if="
            data.rowsWorked == 7 &&
            data.increase === data.increasesDone &&
            data.decreasesDone >= data.decrease
          "
          class="alert alert-success"
        >
          <Icon name="material-symbols:celebration" />
          <span><strong>Jetzt musst du abketten!</strong></span>
        </div>
        <div
          v-else-if="
            data.rowsWorked == 7 &&
            data.increase === data.increasesDone &&
            data.decreasesDone === data.decrease
          "
          class="alert alert-info"
        >
          <Icon name="material-symbols:info" />
          <span><strong>Jetzt die letzten 7 Reihen stricken!</strong></span>
        </div>
        <div v-else-if="data.rowsWorked == 7" class="alert alert-warning">
          <Icon name="material-symbols:warning" />
          <span
            >Zeit für eine
            <strong>{{
              data.increase === data.increasesDone ? "Abnahme" : "Zunahme"
            }}</strong
            >!</span
          >
        </div>

        <div class="text-center space-y-4">
          <h2 class="text-xl font-semibold">Reihenzähler</h2>
          <div class="flex items-center justify-center gap-4">
            <button
              @click="decreaseAlreadyKnittedRows"
              class="btn btn-circle btn-lg btn-secondary shadow-lg hover:shadow-xl transform hover:scale-110 transition-all"
            >
              <Icon name="material-symbols:remove" class="text-2xl" />
            </button>

            <div
              class="w-24 h-24 rounded-3xl bg-gradient-to-r from-primary to-secondary flex items-center justify-center shadow-lg"
            >
              <span class="text-4xl font-bold text-white">{{
                data.rowsWorked
              }}</span>
            </div>

            <button
              @click="increaseAlreadyKnittedRows"
              class="btn btn-circle btn-lg btn-primary shadow-lg hover:shadow-xl transform hover:scale-110 transition-all"
            >
              <Icon name="material-symbols:add" class="text-2xl" />
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
          <div
            class="radial-progress text-primary"
            :style="{
              '--value': progress,
              '--size': '10rem',
              '--thickness': '1rem',
            }"
          >
            <div class="text-center">
              <div class="text-2xl font-bold">
                {{ Math.round(progress) }}%
              </div>
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
            <h3 class="font-bold text-lg text-error mb-4">
              Projekt zurücksetzen?
            </h3>
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
  // Total steps: increases + decreases + final 7 rows
  const totalSteps = (props.data.increase + props.data.decrease) * 8 + 7

  let completedSteps = (props.data.increasesDone + props.data.decreasesDone) * 8

  // If all decreases are done, we're in the final 7 rows phase
  if (props.data.decrease > 0 && props.data.decreasesDone >= props.data.decrease) {
    completedSteps += props.data.rowsWorked
  } else {
    completedSteps += props.data.rowsWorked
  }

  return totalSteps > 0 ? (completedSteps / totalSteps) * 100 : 0
})

const increaseAlreadyKnittedRows = () => {
  const newData = { ...props.data }
  
  if (newData.rowsWorked >= 7) {
    newData.rowsWorked = 0
    // If all increases and decreases are done, don't increment anything (final rows phase)
    if (
      newData.increase === newData.increasesDone &&
      newData.decreasesDone >= newData.decrease
    ) {
      // We're in final rows phase or complete
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