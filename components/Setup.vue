<template>
  <div class="min-h-screen flex items-center justify-center px-4 lg:px-8">
    <div class="w-full max-w-md lg:max-w-2xl xl:max-w-4xl">
      <div class="card bg-base-100 shadow-2xl">
        <div class="card-body p-6 lg:p-12 space-y-8">
          <!-- Header Section -->
          <div class="text-center space-y-6">
            <div
              class="w-20 h-20 lg:w-24 lg:h-24 mx-auto bg-gradient-to-r from-primary to-secondary rounded-3xl flex items-center justify-center"
            >
              <Icon
                name="material-symbols:content-cut"
                class="text-3xl lg:text-4xl text-white"
              />
            </div>
            <div class="space-y-2">
              <h1
                class="text-4xl lg:text-6xl xl:text-7xl font-bold bg-gradient-to-r from-primary to-secondary bg-clip-text text-transparent"
              >
                Knittly
              </h1>
              <p
                class="text-lg lg:text-xl text-base-content/70 max-w-lg mx-auto"
              >
                Dein persönlicher Strick-Assistent für den Sophie Scarf
              </p>
            </div>
          </div>

          <!-- Form Section -->
          <div class="space-y-6 lg:space-y-8">
            <!-- Input Grid for Desktop -->
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 lg:gap-8">
              <div class="form-control space-y-2">
                <label class="label">
                  <span class="label-text font-semibold text-base lg:text-lg"
                    >Anzahl Zunahmen</span
                  >
                </label>
                <input
                  v-model="localData.increase"
                  type="number"
                  min="0"
                  step="1"
                  class="input input-bordered input-lg text-center text-2xl lg:text-3xl font-bold"
                  placeholder="0"
                />
              </div>

              <div class="form-control space-y-2">
                <label class="label">
                  <span class="label-text font-semibold text-base lg:text-lg"
                    >Bereits gemachte Zunahmen</span
                  >
                </label>
                <input
                  v-model="localData.increasesDone"
                  type="number"
                  min="0"
                  step="1"
                  :max="localData.increase"
                  class="input input-bordered input-lg text-center text-2xl lg:text-3xl font-bold"
                  placeholder="0"
                />
              </div>
            </div>

            <!-- Second Row -->
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 lg:gap-8">
              <div
                v-if="
                  localData.increase > 0 &&
                  localData.increasesDone == localData.increase
                "
                class="form-control space-y-2"
              >
                <label class="label">
                  <span class="label-text font-semibold text-base lg:text-lg"
                    >Bereits gemachte Abnahmen</span
                  >
                </label>
                <input
                  v-model="localData.decreasesDone"
                  type="number"
                  min="0"
                  step="1"
                  :max="localData.decrease"
                  class="input input-bordered input-lg text-center text-2xl lg:text-3xl font-bold"
                  placeholder="0"
                />
              </div>

              <div class="form-control space-y-2">
                <label class="label">
                  <span class="label-text font-semibold text-base lg:text-lg"
                    >Reihen im aktuellen Block</span
                  >
                </label>
                <input
                  v-model="localData.rowsWorked"
                  type="number"
                  min="0"
                  step="1"
                  max="7"
                  class="input input-bordered input-lg text-center text-2xl lg:text-3xl font-bold"
                  placeholder="0"
                />
              </div>
            </div>

            <!-- Start Button -->
            <div class="pt-4">
              <button
                v-if="
                  localData.increase > 0 &&
                  localData.increasesDone <= localData.increase &&
                  localData.rowsWorked < 8 &&
                  localData.decreasesDone <= localData.increase &&
                  localData.decreasesDone >= localData.decrease
                "
                @click="handleStartApp"
                class="btn btn-primary btn-lg lg:btn-xl text-lg lg:text-xl font-semibold shadow-2xl hover:shadow-3xl transform hover:scale-105 transition-all duration-300 w-full"
              >
                <Icon
                  name="material-symbols:play-arrow"
                  class="text-2xl lg:text-3xl"
                />
                Los geht's!
              </button>
            </div>
          </div>
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
