<script setup>
import { twMerge } from "tailwind-merge";
import { computed, defineEmits, defineProps } from "vue";

const props = defineProps({
  className: String,
});

const emit = defineEmits(["click"]);

const handleClick = () => {
  emit("click");
};

const buttonClass = computed(() => {
  return twMerge(
    "w-fit border-none rounded-md bg-[#0c7d69] hover:bg-[#2b4246] py-2 px-6 tracking-wider text-white justify-items-center",
    props.className
  );
});
</script>

<template>
  <button @click="handleClick" :class="buttonClass">
    <slot />
  </button>
</template>
