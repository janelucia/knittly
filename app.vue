<template>
  <div class="flex flex-col gap-2 text-primary-content h-full justify-center bg-gradient-to-b from-accent via-base-100 to-base-100">
    <div v-if="!informationSaved" class="h-2/3 flex flex-col gap-4 py-20 px-4">
      <div class="flex flex-col gap-2">
        <h1 class="text-2xl font-bold">Hallo 👋</h1>
        <p>Der Sophie Scarf wird alle 8 Reihen um eine Masche erweitert/ verringert.</p>
      </div>
      <div class="flex flex-col gap-4 h-full justify-evenly ">
        <div class="w-full flex flex-col gap-2">
          <p>Wie viele <span class="text-primary font-bold">Hinzunahmen</span> möchtest du haben?</p>
          <input v-model="data.increase" type="number" min="0" step="1" class="border p-2 rounded w-full text-right" placeholder="Gib die Anzahl der Reihen ein" />
        </div>
        <div v-if="data.increase > 0" class="w-full flex flex-col gap-2">
          <p>Wie viele <span class="text-primary font-bold">Hinzunahmen</span> hast du schon?</p>
          <input v-model="data.increasesDone" type="number" min="0" step="1" :max="data.increase" class="border p-2 rounded w-full text-right" placeholder="Gib die Anzahl der Reihen ein" />
        </div>
        <div class="w-full flex flex-col gap-2">
          <p>Wie viele <span class="text-primary font-bold">Abnahmen</span> möchtest du haben?</p>
          <input v-model="data.decrease" type="number" min="0" step="1" class="border p-2 rounded w-full text-right" placeholder="Gib die Anzahl der Reihen ein" />
        </div>
        <div v-if="data.increase > 0 && data.increasesDone == data.increase" class="w-full flex flex-col gap-2">
          <p>Wie viele <span class="text-primary font-bold">Abnahmen</span> hast du schon?</p>
          <input v-model="data.decreasesDone" type="number" min="0" step="1" :max="data.decrease" class="border p-2 rounded w-full text-right" placeholder="Gib die Anzahl der Reihen ein" />
        </div>
        <div class="w-full flex flex-col gap-2">
          <p>Wie viele <span class="text-primary font-bold">Reihen</span> in der momentanen Hinzunahme/ Abnahme hast du schon?</p>
          <input v-model="data.rowsWorked" type="number" min="0" step="1" max="7" class="border p-2 rounded w-full text-right" placeholder="Gib die Anzahl der Reihen ein" />
        </div>
        <button
            v-if="(data.increase > 0 && data.increasesDone < data.increase) || (data.decrease > 0 && data.decreasesDone < data.decrease)"
            class="btn btn-secondary btn-md w-1/2 self-center text-base-100"
            @click="save"
        >
          Berechnen
        </button>

        <p v-else-if="(data.increase > 0 && data.increasesDone == data.increase) && (data.decrease > 0 && data.decreasesDone == data.decrease)" class="text-secondary-content text-center font-bold text-4xl">
          Du bist fertig! 🎉
        </p>

      </div>
    </div>
    <div v-else-if="done" class="flex flex-col justify-center items-center gap-4 h-screen">
      <h1 class="text-2xl font-bold">Herzlichen Glückwunsch! 🎉</h1>
      <p>Du hast dein Projekt erfolgreich abgeschlossen!</p>
      <button class="btn btn-secondary btn-md w-1/2 self-center text-base-100" @click="reset">
        Neues Projekt starten
      </button>
    </div>
    <div v-else class="flex flex-col justify-between gap-8 py-20 px-4">
      <div class="flex flex-col gap-2">
        <h1 class="text-2xl font-bold">Dein Sophie Scarf</h1>
        <p>Du hast <span class="text-secondary-content font-bold">{{ data.increase + data.decrease }}</span> Reihen geplant.</p>
        <p>Du befindest dich gerade in der <span class="text-secondary-content font-bold">{{ data.increase == data.increasesDone ? 'Abnahme' : 'Zunahme'}}</span>.</p>
        <div class="flex gap-2 items-center">
          <div class="flex gap-2">
            <div class="border-4 border-accent rounded-xl p-4 text-center flex flex-col">
              <p class="text-xl font-bold">{{ data.increase == data.increasesDone ? data.decrease : data.increase }}</p>
              <p>{{data.increase == data.increasesDone ? 'Abnahmen' : 'Zunahmen'}} Total</p>
            </div>
            <div class="border-4 border-accent rounded-xl p-4 text-center flex flex-col">
              <p class="text-xl font-bold">{{ data.increase == data.increasesDone ? data.decreasesDone + 1 : data.increasesDone + 1 }}</p>
              <p>Momentane {{ data.increase == data.increasesDone ? 'Abnahmen' : 'Zunahmen' }}</p>
            </div>
          </div>
          <div class="w-fit flex flex-col items-center gap-2">
            <div
                class="radial-progress text-primary"
                :style="{ '--value': progress, '--size': '8rem', '--thickness': '1rem' }"
                :aria-valuenow="progress"
                role="progressbar"
            >
              {{ progress.toFixed(2) }} %
            </div>
            <p class="text-center text-sm">Projekt Fortschritt</p>
          </div>
        </div>
      </div>
      <div class="flex items-center justify-center flex-col">
        <div v-if="data.rowsWorked == 7" class="alert alert-accent w-full">
          Du musst jetzt eine <span class="text-primary font-bold">{{ data.increase === data.increasesDone ? 'Abnahme' : 'Zunahme'}}</span> machen!
        </div>
        <button class="text-6xl text-primary" @click="increaseAlreadyKnittedRows">
          <Icon name="material-symbols:keyboard-arrow-up-rounded" />
        </button>
        <div class="text-center text-4xl font-bold w-24 h-24 rounded-xl bg-primary text-base-100 flex items-center justify-center">
          {{data.rowsWorked}}
        </div>
        <button class="text-6xl text-primary rotate-180" @click="decreaseAlreadyKnittedRows">
          <Icon name="material-symbols:keyboard-arrow-up-rounded" />
        </button>
      </div>
      <div class="grid grid-cols-2 gap-4 text-left font-mono text-lg leading-tight">
        <div>
          <h2 class="text-lg font-semibold mb-2">Zunahmen</h2>
          <div v-for="(block, index) in increaseBlocks" :key="'inc-block-' + index">
            {{ block }}
          </div>
        </div>
        <div>
          <h2 class="text-lg font-semibold mb-2">Abnahmen</h2>
          <div v-for="(block, index) in decreaseBlocks" :key="'dec-block-' + index">
            {{ block }}
          </div>
        </div>
      </div>
      <div class="flex flex-col gap-2">
        <label for="reset-modal" class="btn btn-outline btn-error btn-md">
          Projekt zurücksetzen
        </label>

        <input type="checkbox" id="reset-modal" class="modal-toggle" />

        <div class="modal">
          <div class="modal-box">
            <h3 class="font-bold text-lg text-error">Projekt wirklich zurücksetzen?</h3>
            <p class="py-4">Alle eingegebenen Daten gehen dauerhaft verloren.</p>

            <label class="flex items-center gap-2 mb-4">
              <input type="checkbox" v-model="confirmReset" class="checkbox checkbox-error" />
              <span>Ich bin mir sicher.</span>
            </label>

            <div class="modal-action">
              <label for="reset-modal" class="btn">Abbrechen</label>

              <label
                  for="reset-modal"
                  class="btn btn-error text-base-100"
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

const save = () => {
  localStorage.setItem('knittingData', JSON.stringify(data));
  informationSaved.value = true;
};

const loadData = () => {
  const savedData = localStorage.getItem('knittingData');
  if (savedData) {
    Object.assign(data, JSON.parse(savedData));
    informationSaved.value = true;
  }
};

const progress = computed(() => {
  console.log(data.increasesDone);
  return ((data.increasesDone + data.decreasesDone) / (data.increase + data.decrease)) * 100;
})

const increaseAlreadyKnittedRows = () => {
  if (data.rowsWorked >= 7) {
    data.rowsWorked = 0;
    if (data.increase == data.increasesDone) {
      data.decreasesDone++;
    } else {
      data.increasesDone++;
    }
  } else {
    data.rowsWorked++;
  }
  save();
};

const decreaseAlreadyKnittedRows = () => {
  if (data.rowsWorked > 0) {
    data.rowsWorked--;
    save();
  }
};


const renderBlock = (rows: number): string => {
  const str = Array.from({ length: 8 }, (_, i) =>
      i < rows ? 'I' : ''
  )
  return str.slice(0, 4).join('') + ' ' + str.slice(4).join('')
}

const increaseBlocks = computed(() => {
  const blocks: string[] = []
  for (let i = 0; i < data.increase; i++) {
    const isCurrent = i === data.increasesDone
    const isDone = i < data.increasesDone
    const rows = isDone ? 8 : isCurrent ? data.rowsWorked : 0
    blocks.push(renderBlock(rows))
  }
  return blocks
})

const decreaseBlocks = computed(() => {
  const blocks: string[] = []
  for (let i = 0; i < data.decrease; i++) {
    const isCurrent = i === data.decreasesDone
    const isDone = i < data.decreasesDone
    const rows = isDone ? 8 : isCurrent && data.increase === data.increasesDone ? data.rowsWorked : 0
    blocks.push(renderBlock(rows))
  }
  return blocks
})

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
}

watch(
    () => data.decreasesDone,
    (newValue) => {
      console.log(data.increase, data.increasesDone, data.decrease, newValue);
      done.value = data.increase === data.increasesDone && newValue >= data.decrease;
    }
);

onMounted(
  () => {
    loadData();
  }
)
</script>
