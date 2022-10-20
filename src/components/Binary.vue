<script setup lang="ts">
import { ref } from "vue";
import Input from "./atoms/Input.vue";
import Button from "./atoms/Button.vue";

const DEFAULT_YES = "Yes";
const DEFAULT_NO = "No";

const result = ref<string | null>(null);
const question = ref<string>("");
const yes = ref<string>(DEFAULT_YES);
const no = ref<string>(DEFAULT_NO);

const choose = () => {
  result.value = Math.random() > 0.5 ? yes.value : no.value;
};
</script>

<template>
  <div class="flex flex-col w-full max-w-screen-sm">
    <Transition
      mode="out-in"
      enter-active-class="transition ease-out delay-250"
      leave-active-class="transition ease-out delay-250"
      enter-from-class="opacity-0 translate-y-[10px]"
      leave-to-class="opacity-0 translate-y-[-10px]"
    >
      <div v-if="!result" class="flex flex-col gap-6">
        <div>
          <Input
            v-model="question"
            id="question-field"
            label="I need you to decide for me."
            append="?"
            preppend="Should I"
          />
        </div>
        <div class="flex gap-4">
          <Input
            v-model="yes"
            id="yes-field"
            label="Positive answer:"
            class="w-full"
          />
          <Input
            v-model="no"
            id="no-field"
            label="Negative answer:"
            class="w-full"
          />
        </div>
        <Button @click="choose" bnt-type="primaryFill" :disabled="!question">
          Show me my destiny!
        </Button>
      </div>
      <div v-else class="flex flex-col gap-4">
        <div class="flex flex-col items-center gap-4">
          <div class="text-lg">Should I {{ question }}?</div>
          <Transition
            appear
            appear-from-class="opacity-0"
            appear-active-class="transition ease-in delay-[2000ms]"
          >
            <div class="text-2xl h-10">
              {{ result }}
            </div>
          </Transition>
        </div>
        <div class="flex items-center text-sm">
          <div>Nothing is set on stone. Really.</div>
          <Button @click="result = null" bnt-type="primaryGhost" class="pl-1">
            Want to try again?
          </Button>
        </div>
      </div>
    </Transition>
  </div>
</template>
