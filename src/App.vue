<script setup lang="ts">
import { ref } from "vue";
import Binary from "./components/Binary.vue";
import Multiple from "./components/Multiple.vue";

const tabs = [
  { tab: "Yes or No", component: Binary },
  { tab: "Multiple options", component: Multiple },
];

const currentTab = ref(0);
</script>

<template>
  <header class="flex items-center w-full">
    <nav class="mx-auto flex">
      <button
        v-for="(tab, index) in tabs"
        :key="index"
        @click="currentTab = index"
        class="p-2 text-center border-b-2"
        :class="currentTab === index ? 'border-pink-600' : 'border-transparent'"
      >
        {{ tab.tab }}
      </button>
    </nav>
  </header>
  <main class="flex flex-col items-center justify-center flex-1">
    <Transition
      mode="out-in"
      enter-active-class="transition ease-in delay-250"
      leave-active-class="transition ease-out delay-250"
      enter-from-class="opacity-0 translate-x-[200px]"
      leave-to-class="opacity-0 translate-x-[-200px]"
    >
      <component :is="tabs[currentTab].component"></component>
    </Transition>
  </main>
  <footer class="flex items-center justify-center h-12">
    <div>fmmendes.com</div>
  </footer>
</template>
