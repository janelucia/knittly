<template>
  <div
    class="min-h-screen bg-base-200/30 flex items-center justify-center p-4 lg:p-6"
  >
    <div class="w-full max-w-2xl">
      <div class="card bg-base-100 shadow-lg border border-base-200">
        <div class="card-body p-6 lg:p-8 text-center space-y-6">
          <!-- Celebration Section -->
          <div class="space-y-4">
            <div class="text-6xl lg:text-8xl animate-bounce">🎉</div>
            <div class="space-y-2">
              <h1
                class="text-3xl lg:text-4xl font-bold bg-gradient-to-r from-primary to-secondary bg-clip-text text-transparent"
              >
                Herzlichen Glückwunsch!
              </h1>
              <p class="text-lg text-base-content/70">
                Du hast deinen Sophie Scarf erfolgreich abgeschlossen!
              </p>
            </div>
          </div>

          <!-- Action Buttons -->
          <div class="space-y-3">
            <button
              @click="handleReset"
              class="btn btn-primary btn-lg w-full shadow-md hover:shadow-lg transition-all duration-200"
            >
              <Icon name="material-symbols:refresh" class="text-xl" />
              Neues Projekt starten
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const emit = defineEmits<{
  reset: [];
}>();

const handleReset = () => {
  emit("reset");
};
</script>
