<template>
  <button :class="buttonClasses" @click="handleClick">
    <span v-if="icon" class="mr-2">
      <component :is="icon" />
    </span>
    {{ label }}
  </button>
</template>

<script setup>
import { computed } from "vue";
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  label: {
    type: String,
    required: true,
  },
  color: {
    type: String,
    default: "blue",
  },
  size: {
    type: String,
    default: "medium",
  },
  icon: {
    type: [Object, Function],
    default: null,
  },
});

const emit = defineEmits(["click"]);

const colorClasses = {
  red: "bg-red-500 hover:bg-red-600 text-white",
  blue: "bg-blue-500 hover:bg-blue-600 text-white",
  green: "bg-green-500 hover:bg-green-600 text-white",
  gray: "bg-gray-500 hover:bg-gray-600 text-white",
};

const sizeClasses = {
  small: "px-3 py-1 text-sm",
  medium: "px-4 py-2 text-base",
  large: "px-6 py-3 text-lg",
};

const buttonClasses = computed(() => {
  return [
    "inline-flex items-center font-medium rounded transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2",
    colorClasses[props.color] || colorClasses.blue,
    sizeClasses[props.size] || sizeClasses.medium,
  ].join(" ");
});

const handleClick = (event) => {
  emit("click", event);
};
</script>
