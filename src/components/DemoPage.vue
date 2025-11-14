<template>
  <div class="p-6 space-y-12">
    <h1 class="text-2xl font-bold mb-4">Demo Buttons</h1>
    <div class="space-y-4">
      <div v-for="color in colors" :key="color" class="space-y-2">
        <h2 class="font-semibold">{{ color }}</h2>
        <div class="flex flex-wrap gap-4">
          <Button
            v-for="size in sizes"
            :key="size + color + '-no-icon'"
            :label="size + ' Button'"
            :color="color"
            :size="size"
            @click="handleButtonClick(color, size, false)"
          />
          <Button
            v-for="size in sizes"
            :key="size + color + '-icon'"
            :label="size + ' Button'"
            :color="color"
            :size="size"
            :icon="DemoIcon"
            @click="handleButtonClick(color, size, true)"
          />
        </div>
      </div>
    </div>

    <h1 class="text-2xl font-bold mb-4">Demo SearchableDropdown</h1>
    <div class="space-y-6">
      <div class="flex flex-col justify-center items-center">
        <h2 class="font-semibold mb-2">Single selection</h2>
        <SearchableDropdown :items="dropdownData1" v-model="selectedSingle" />
      </div>

      <div class="flex flex-col justify-center items-center">
        <h2 class="font-semibold mb-2">Multiple selection</h2>
        <SearchableDropdown
          :items="dropdownData2"
          v-model="selectedMultiple"
          multiple
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Button from "./Button.vue";
import SearchableDropdown from "./SearchableDropdown.vue";
import { defineComponent, h } from "vue";

const DemoIcon = defineComponent({
  name: "DemoIcon",
  render() {
    return h(
      "svg",
      {
        class: "w-4 h-4",
        fill: "none",
        stroke: "currentColor",
        "stroke-width": 2,
        viewBox: "0 0 24 24",
        "stroke-linecap": "round",
        "stroke-linejoin": "round",
      },
      [h("circle", { cx: 12, cy: 12, r: 10 }), h("path", { d: "M12 8v4l3 3" })]
    );
  },
});

const colors = ["red", "blue", "green", "gray"];
const sizes = ["small", "medium", "large"];

const dropdownData1 = [
  { value: "1", label: "Option 1" },
  { value: "2", label: "Option 2" },
  { value: "3", label: "Option 3" },
];

const dropdownData2 = [
  { value: "a", label: "Alpha" },
  { value: "b", label: "Beta" },
  { value: "c", label: "Gamma" },
  { value: "d", label: "Delta" },
];

const selectedSingle = ref(null);
const selectedMultiple = ref([]);

function handleButtonClick(color, size, withIcon) {
  console.log("Button clicked:", color, size, withIcon);
}
</script>
