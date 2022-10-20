<script setup lang="ts">
defineProps<{
  modelValue: string;
  id: string;
  label: string;
  append?: string;
  preppend?: string;
}>();

defineEmits<{
  (e: "update:modelValue", value: string): void;
}>();
</script>

<template>
  <div class="flex flex-col">
    <label :for="id">{{ label }}</label>
    <div class="flex w-full items-center">
      <div v-show="preppend" class="whitespace-nowrap">{{ preppend }}</div>
      <input
        :value="modelValue"
        @input="
          $emit('update:modelValue', ($event.target as HTMLInputElement).value)
        "
        :id="id"
        :name="id"
        class="p-2 w-full bg-transparent border-b border-l-4 border-b-pink-600 border-l-transparent focus:border-l-pink-600 focus:outline-none"
      />
      <div v-show="append">{{ append }}</div>
    </div>
  </div>
</template>
