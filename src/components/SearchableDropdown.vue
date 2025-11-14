<template>
  <div class="relative w-64 text-black">
    <!-- Input field -->
    <div
      class="flex flex-wrap items-center border rounded px-2 py-1 bg-white"
      @click="open = true"
    >
      <template v-if="multiple">
        <span
          v-for="(item, index) in selected"
          :key="item.id || item.value"
          class="flex items-center bg-blue-100 text-blue-700 px-2 py-1 mr-1 mb-1 rounded"
        >
          {{ item.label }}
          <button
            type="button"
            @click.stop="removeItem(index)"
            class="ml-1 text-blue-500 hover:text-blue-700"
          >
            ✕
          </button>
        </span>
      </template>

      <input
        type="text"
        v-model="search"
        @focus="open = true"
        @keydown.down.prevent="highlightNext"
        @keydown.up.prevent="highlightPrev"
        @keydown.enter.prevent="selectHighlighted"
        :placeholder="multiple && selected.length ? '' : 'Виберіть...'"
        class="flex-1 outline-none"
      />

      <!-- Clear all button -->
      <button
        v-if="!multiple && selected"
        @click.stop="clearSelection"
        class="ml-2 text-gray-500 hover:text-gray-700"
      >
        ✕
      </button>
    </div>

    <!-- Dropdown list -->
    <ul
      v-if="open"
      class="absolute z-10 mt-1 w-full bg-white border rounded shadow max-h-60 overflow-auto"
    >
      <li
        v-for="(item, index) in filteredItems"
        :key="item.id || item.value"
        @click.stop="selectItem(item)"
        @mouseenter="highlightedIndex = index"
        :class="[
          'cursor-pointer px-2 py-1',
          highlightedIndex === index ? 'bg-blue-500 text-white' : ''
        ]"
      >
        <slot name="item" :item="item">
          {{ item.label }}
        </slot>
      </li>

      <li v-if="filteredItems.length === 0" class="px-2 py-1 text-gray-500">
        Нічого не знайдено
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";

const props = defineProps({
  items: {
    type: Array,
    required: true,
  },
  modelValue: {
    type: [Array, Object, String, Number, null],
    default: null,
  },
  multiple: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits(["update:modelValue"]);

const search = ref("");
const open = ref(false);
const highlightedIndex = ref(0);
const selected = ref(props.multiple ? (props.modelValue || []) : props.modelValue);

const filteredItems = computed(() => {
  return props.items.filter(
    (item) =>
      item.label.toLowerCase().includes(search.value.toLowerCase()) &&
      (!props.multiple || !selected.value.some((s) => s.value === item.value))
  );
});

watch(
  () => props.modelValue,
  (val) => {
    selected.value = props.multiple ? val || [] : val;
    if (!props.multiple && val) search.value = val.label || val;
  }
);

function selectItem(item) {
  if (props.multiple) {
    selected.value.push(item);
    emit("update:modelValue", selected.value);
    search.value = "";
    highlightedIndex.value = 0;
  } else {
    selected.value = item;
    search.value = item.label;
    emit("update:modelValue", item);
    open.value = false;
    highlightedIndex.value = 0;
  }
}

function removeItem(index) {
  selected.value.splice(index, 1);
  emit("update:modelValue", selected.value);
}

function clearSelection() {
  selected.value = null;
  search.value = "";
  emit("update:modelValue", null);
}

function highlightNext() {
  if (highlightedIndex.value < filteredItems.value.length - 1) highlightedIndex.value++;
}
function highlightPrev() {
  if (highlightedIndex.value > 0) highlightedIndex.value--;
}
function selectHighlighted() {
  const item = filteredItems.value[highlightedIndex.value];
  if (item) selectItem(item);
}

function handleClickOutside(event) {
  if (!event.target.closest(".relative")) {
    open.value = false;
  }
}
document.addEventListener("click", handleClickOutside);
</script>
