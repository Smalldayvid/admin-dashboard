<script setup>
import { defineEmits, defineProps } from "vue";

defineProps({
  message: {
    type: String,
    default: "Message",
  },
  modelValue: String,
});

const emit = defineEmits(["update:modelValue"]);

const emitInput = (event) => {
  emit("update:modelValue", event.target.value);
};
</script>

<template>
  <div class="space-y-1">
    <label class="w-2/5">{{ message }} :</label>
    <textarea
      class="w-full rounded-md border border-[#87a3a6] focus:outline-none px-2 py-4"
      :value="modelValue"
      @input="emitInput"
    ></textarea>
  </div>
</template>
