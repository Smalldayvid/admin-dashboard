<script setup>
import { defineEmits, defineProps } from "vue";

defineProps({
  label: {
    type: String,
    default: "Email",
  },
  type: {
    type: String,
    default: "text",
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
    <label class="w-full">{{ label }} :</label>
    <input
      class="flex w-full rounded-md border border-[#87a3a6] focus:outline-none px-2 py-2"
      :type="type"
      :value="modelValue"
      @input="emitInput"
    />
  </div>
</template>
