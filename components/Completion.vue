<template>
  <div class="min-h-screen flex items-center justify-center px-6">
    <div class="card w-full bg-base-100 shadow-2xl">
      <div class="card-body text-center space-y-6">
        <div class="text-8xl">🎉</div>
        <h1
          class="text-3xl font-bold bg-gradient-to-r from-primary to-secondary bg-clip-text text-transparent"
        >
          Herzlichen Glückwunsch!
        </h1>
        <p class="text-lg text-base-content/70">
          Du hast deinen Sophie Scarf erfolgreich abgeschlossen!
        </p>
        <button @click="handleReset" class="btn btn-primary btn-lg w-full">
          <Icon name="material-symbols:refresh" class="text-xl" />
          Neues Projekt starten
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const emit = defineEmits<{
  reset: []
}>()

const handleReset = () => {
  emit('reset')
}
</script>