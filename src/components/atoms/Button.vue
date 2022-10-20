<script setup lang="ts">
import { computed } from "vue";

const BTN_TYPE = {
  primaryFill:
    "border border-pink-600 bg-pink-600 text-white hover:bg-pink-800 hover:border-pink-800 focus:outline-pink-800",
  primaryOutline:
    "border border-pink-600 text-pink-600 hover:bg-pink-600 hover:border-pink-600 hover:text-white focus:outline-pink-600",
  primaryGhost:
    "border border-transparent text-pink-600 hover:text-pink-600 hover:underline focus:outline-pink-600",
  secondaryFill:
    "border border-cyan-500 bg-cyan-500 text-white hover:bg-cyan-700 hover:border-cyan-700 hover:text-white focus:outlink-cyan-700",
  secondaryOutline:
    "border border-cyan-500 text-cyan-500 hover:bg-cyan-500 hover:border-cyan-500 hover:text-white focus:outlink-cyan-700",
  secondaryGhost:
    "border border-transparent text-cyan-500 hover:bg-cyan-500 hover:border-cyan-500 hover:text-white focus:outlink-cyan-500",
};

const props = defineProps<{
  bntType?: keyof typeof BTN_TYPE;
  disabled?: boolean;
}>();

const className = computed(() => BTN_TYPE[props.bntType || "primaryFill"]);
</script>

<template>
  <button
    :class="className"
    :disabled="disabled || false"
    class="p-2 transition-colors disabled:opacity-70 disabled:pointer-events-none"
  >
    <slot></slot>
  </button>
</template>
