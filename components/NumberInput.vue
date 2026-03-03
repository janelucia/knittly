<template>
  <div class="form-control space-y-2">
    <label class="label">
      <span class="label-text font-semibold text-base lg:text-lg">{{
        label
      }}</span>
    </label>
    <input
      :value="modelValue"
      @input="
        emit(
          'update:modelValue',
          Number(($event.target as HTMLInputElement).value),
        )
      "
      type="number"
      min="0"
      step="1"
      :max="max"
      class="input input-bordered w-full lg:input-lg text-center text-2xl lg:text-3xl font-bold"
      placeholder="0"
    />
  </div>
</template>

<script setup lang="ts">
defineProps<{
  label: string;
  modelValue: number;
  max?: number;
}>();

const emit = defineEmits<{
  "update:modelValue": [value: number];
}>();
</script>
