<script setup lang="ts">
import { defineProps, defineEmits } from 'vue'

defineProps<{
  label?: string
  placeholder?: string
  modelValue: string
  type: string
  error?: string
}>()

const emit = defineEmits(['update:modelValue'])

function handleInput(event: Event) {
  const value = (event.target as HTMLInputElement).value
  emit('update:modelValue', value)
}
</script>

<template>
  <div>
    <label
      v-if="label"
      :class="{
        'text-red-700': !!error,
      }"
      class="block mb-2 text-sm font-medium text-gray-900"
      >{{ label }}</label
    >
    <input
      @input="handleInput"
      :type="type"
      :placeholder="placeholder"
      :value="modelValue"
      :class="{
        'bg-red-50 border border-red-500 text-red-900 placeholder-red-700': !!error,
      }"
      class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
    />
    <div v-if="error" class="mt-2 text-sm text-red-600">{{ error }}</div>
  </div>
</template>
