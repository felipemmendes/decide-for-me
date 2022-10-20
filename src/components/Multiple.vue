<script setup lang="ts">
import { reactive, ref } from "vue";
import Input from "./atoms/Input.vue";
import Button from "./atoms/Button.vue";

let id = 0;
const result = ref<string | null>(null);
const calculating = ref<boolean>(false);
const state = reactive<{ options: { value: string; id: number }[] }>({
  options: [
    { value: "Option 1", id: id++ },
    { value: "Option 2", id: id++ },
  ],
});

const updateOption = (value: string, id: number) => {
  const option = state.options.find((o) => o.id === id);
  if (!option) return;

  option.value = value;
};

const addOption = () => {
  state.options.push({ value: "New option", id: id++ });
};

const removeOption = (id: number) => {
  state.options = state.options.filter((o) => o.id !== id);
};

const choose = () => {
  result.value =
    state.options[Math.floor(Math.random() * state.options.length)].value;
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
        <div>I need you to pick an option for me:</div>
        <div class="flex flex-col gap-4">
          <template v-for="({ value, id }, index) in state.options" :key="id">
            <div class="flex gap-2 justify-between items-end">
              <Input
                :modelValue="value"
                @update:modelValue="(v) => updateOption(v, id)"
                :id="`yes-field-${id}`"
                :label="`Option ${index + 1}:`"
                class="w-full"
              />
              <Button @click="removeOption(id)" class="">x</Button>
            </div>
          </template>
          <Button @click="addOption" bnt-type="primaryOutline"
            >Add option</Button
          >
        </div>
        <Button
          @click="choose"
          bnt-type="primaryFill"
          :disabled="state.options.length < 2"
        >
          Show me my destiny!
        </Button>
      </div>
      <div v-else class="flex flex-col gap-4">
        <div class="flex flex-col items-center gap-4">
          <div class="text-lg">You have to choose:</div>
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
