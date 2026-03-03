<template>
  <div class="flex justify-between">
    <span class="text-base-content/70">{{ label }}:</span>
    <span class="font-medium">{{ done }}/{{ total }}</span>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  label: string;
  done: number;
  total: number;
}>();
</script>
