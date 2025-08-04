<template>
  <div class="min-h-screen flex items-center justify-center px-6">
    <div class="card w-full bg-base-100 shadow-2xl">
      <div class="card-body space-y-6">
        <div class="text-center space-y-4">
          <div
            class="w-16 h-16 mx-auto bg-gradient-to-r from-primary to-secondary rounded-2xl flex items-center justify-center"
          >
            <Icon
              name="material-symbols:content-cut"
              class="text-2xl text-white"
            />
          </div>
          <h1
            class="text-3xl font-bold bg-gradient-to-r from-primary to-secondary bg-clip-text text-transparent"
          >
            Knittly
          </h1>
          <p class="text-base-content/70">
            Dein persönlicher Strick-Assistent für den Sophie Scarf
          </p>
        </div>

        <div class="space-y-4">
          <div
            class="form-control flex flex-col sm:flex-row justify-between gap-2"
          >
            <label class="label">
              <span class="label-text font-semibold">Anzahl Zunahmen</span>
            </label>
            <input
              v-model="localData.increase"
              type="number"
              min="0"
              step="1"
              class="input input-bordered text-center text-xl font-bold w-full"
              placeholder="0"
            />
          </div>

          <div
            v-if="localData.increase > 0"
            class="form-control flex flex-col sm:flex-row justify-between gap-2"
          >
            <label class="label">
              <span class="label-text font-semibold"
                >Bereits gemachte Zunahmen</span
              >
            </label>
            <input
              v-model="localData.increasesDone"
              type="number"
              min="0"
              step="1"
              :max="localData.increase"
              class="input input-bordered text-center text-xl font-bold w-full"
              placeholder="0"
            />
          </div>

          <div
            v-if="
              localData.increase > 0 &&
              localData.increasesDone == localData.increase
            "
            class="form-control flex flex-col sm:flex-row justify-between gap-2"
          >
            <label class="label">
              <span class="label-text font-semibold"
                >Bereits gemachte Abnahmen</span
              >
            </label>
            <input
              v-model="localData.decreasesDone"
              type="number"
              min="0"
              step="1"
              :max="localData.decrease"
              class="input input-bordered text-center text-xl font-bold w-full"
              placeholder="0"
            />
          </div>

          <div
            class="form-control flex flex-col sm:flex-row justify-between gap-2"
          >
            <label class="label">
              <span class="label-text font-semibold"
                >Reihen im aktuellen Block</span
              >
            </label>
            <input
              v-model="localData.rowsWorked"
              type="number"
              min="0"
              step="1"
              max="7"
              class="input input-bordered text-center text-xl font-bold w-full"
              placeholder="0"
            />
          </div>

          <button
            v-if="
              localData.increase > 0 &&
              localData.increasesDone >= localData.increase &&
              localData.rowsWorked < 8 &&
              localData.decreasesDone < localData.increase &&
              localData.decreasesDone >= localData.decrease
            "
            @click="handleStartApp"
            class="btn btn-primary btn-lg w-full text-white font-semibold shadow-lg hover:shadow-xl transform hover:scale-105 transition-all duration-200"
          >
            <Icon name="material-symbols:play-arrow" class="text-xl" />
            Los geht's!
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
interface KnittingData {
  rowsWorked: number;
  increasesDone: number;
  decreasesDone: number;
  increase: number;
  decrease: number;
}

const emit = defineEmits<{
  startApp: [data: KnittingData];
}>();

const localData = reactive<KnittingData>({
  rowsWorked: 0,
  increasesDone: 0,
  decreasesDone: 0,
  increase: 0,
  decrease: 0,
});

const handleStartApp = () => {
  // Set decrease equal to increase (Sophie Scarf pattern)
  localData.decrease = localData.increase;

  // Emit the data to parent component
  emit("startApp", { ...localData });
};

// Load existing data if available
const loadData = () => {
  const savedData = localStorage.getItem("knittingData");
  if (savedData) {
    Object.assign(localData, JSON.parse(savedData));
  }
};

onMounted(() => {
  loadData();
});
</script>
