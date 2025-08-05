<template>
  <div class="min-h-screen bg-base-200/30 flex items-center justify-center p-4 lg:p-6">
    <div class="w-full max-w-2xl">
      <div class="card bg-base-100 shadow-lg border border-base-200">
        <div class="card-body p-6 lg:p-8 text-center space-y-6">
          
          <!-- Celebration Section -->
          <div class="space-y-4">
            <div class="text-6xl lg:text-8xl animate-bounce">🎉</div>
            <div class="space-y-2">
              <h1 class="text-3xl lg:text-4xl font-bold bg-gradient-to-r from-primary to-secondary bg-clip-text text-transparent">
                Herzlichen Glückwunsch!
              </h1>
              <p class="text-lg text-base-content/70">
                Du hast deinen Sophie Scarf erfolgreich abgeschlossen!
              </p>
            </div>
          </div>

          <!-- Achievement Cards -->
          <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
            <div class="card bg-gradient-to-br from-success/10 to-success/5 border border-success/20">
              <div class="card-body p-4 text-center">
                <Icon name="material-symbols:check-circle" class="text-2xl text-success mx-auto mb-2" />
                <div class="text-sm font-semibold">Projekt</div>
                <div class="text-xs text-base-content/70">Abgeschlossen</div>
              </div>
            </div>
            <div class="card bg-gradient-to-br from-primary/10 to-primary/5 border border-primary/20">
              <div class="card-body p-4 text-center">
                <Icon name="material-symbols:trending-up" class="text-2xl text-primary mx-auto mb-2" />
                <div class="text-sm font-semibold">Fortschritt</div>
                <div class="text-xs text-base-content/70">100%</div>
              </div>
            </div>
            <div class="card bg-gradient-to-br from-secondary/10 to-secondary/5 border border-secondary/20">
              <div class="card-body p-4 text-center">
                <Icon name="material-symbols:star" class="text-2xl text-secondary mx-auto mb-2" />
                <div class="text-sm font-semibold">Erfahrung</div>
                <div class="text-xs text-base-content/70">+Level!</div>
              </div>
            </div>
          </div>

          <!-- Next Steps -->
          <div class="card bg-base-200/50">
            <div class="card-body p-4">
              <h3 class="text-base font-semibold mb-3 flex items-center gap-2 justify-center">
                <Icon name="material-symbols:lightbulb" class="text-primary" />
                Was als nächstes?
              </h3>
              <div class="text-sm text-base-content/80 space-y-2">
                <div class="flex items-center gap-2">
                  <div class="w-2 h-2 bg-primary rounded-full"></div>
                  <span>Teile dein fertiges Werk in sozialen Medien</span>
                </div>
                <div class="flex items-center gap-2">
                  <div class="w-2 h-2 bg-secondary rounded-full"></div>
                  <span>Starte ein neues, herausforderndes Projekt</span>
                </div>
                <div class="flex items-center gap-2">
                  <div class="w-2 h-2 bg-accent rounded-full"></div>
                  <span>Dokumentiere deine Lernerfahrungen</span>
                </div>
              </div>
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
            <div class="flex gap-2">
              <button class="btn btn-outline btn-sm flex-1">
                <Icon name="material-symbols:share" />
                Teilen
              </button>
              <button class="btn btn-outline btn-sm flex-1">
                <Icon name="material-symbols:photo-camera" />
                Foto machen
              </button>
            </div>
          </div>
        </div>
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