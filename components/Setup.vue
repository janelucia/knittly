<template>
  <div class="min-h-screen flex items-center justify-center px-4 lg:px-8">
    <div class="w-full max-w-md lg:max-w-2xl xl:max-w-4xl">
      <div class="card bg-base-100 shadow-2xl">
        <div class="card-body p-6 lg:p-12 space-y-8">
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

          <div class="space-y-6 lg:space-y-8">
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 lg:gap-8">
              <NumberInput label="Anzahl Zunahmen" v-model="localData.increase" />
              <NumberInput label="Bereits gemachte Zunahmen" v-model="localData.increasesDone" :max="localData.increase" />
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 lg:gap-8">
              <NumberInput
                v-if="localData.increase > 0 && localData.increasesDone == localData.increase"
                label="Bereits gemachte Abnahmen"
                v-model="localData.decreasesDone"
                :max="localData.decrease"
              />
              <NumberInput label="Reihen im aktuellen Block" v-model="localData.rowsWorked" :max="7" />
            </div>

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
import type { KnittingData } from '~/types/knitting'

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
  localData.decrease = localData.increase;
  emit("startApp", { ...localData });
};
</script>
