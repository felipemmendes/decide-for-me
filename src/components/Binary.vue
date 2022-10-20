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
const calculating = ref<boolean>(false);

const choose = () => {
  result.value = Math.random() > 0.5 ? yes.value : no.value;
  calculating.value = false;
};

const repeat = () => {
  calculating.value = true;
  setTimeout(() => {
    choose();
  }, 500);
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
            mode="out-in"
            appear
            appear-from-class="blur-xl"
            appear-active-class="transition ease-linear duration-[2000ms]"
            enter-from-class="blur-xl"
            enter-active-class="transition ease-linear duration-[2000ms]"
            leave-to-class="blur-xl"
            leave-active-class="transition ease-linear duration-[500ms]"
          >
            <div class="text-2xl h-10" v-if="!calculating">
              {{ result }}
            </div>
            <div class="h-10" v-else></div>
          </Transition>
        </div>
        <div class="flex items-center justify-center">
          <Button @click="repeat" bnt-type="primaryFill">Try again</Button>
        </div>
        <div class="flex flex-col items-center text-sm gap-0">
          <div>
            Want to
            <Button
              @click="
                calculating = false;
                result = null;
              "
              bnt-type="primaryGhost"
              class="pl-0"
            >
              change options?
            </Button>
          </div>
        </div>
      </div>
    </Transition>
  </div>
</template>
