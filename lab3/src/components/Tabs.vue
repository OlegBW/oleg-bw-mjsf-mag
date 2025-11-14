<script setup>
import { ref, provide, computed } from 'vue'

const props = defineProps({
  modelValue: {
    type: String,
    required: true
  },
  variant: {
    type: String,
    default: 'underline',
    validator: (value) => ['underline', 'pills', 'boxed'].includes(value)
  }
})

const emit = defineEmits(['update:modelValue'])

const tabs = ref([])

// Compute container classes based on variant
const tabsContainerClass = computed(() => {
  const base = 'flex gap-2 border-b border-gray-300 dark:border-gray-600'
  
  switch (props.variant) {
    case 'pills':
      return 'border-none'
    case 'boxed':
      return 'gap-1 border-none'
    default:
      return base
  }
})

// Compute button classes for each tab
function getTabButtonClass(slug) {
  const isActive = slug === props.modelValue
  
  if (props.variant === 'pills') {
    return [
      'px-4 py-2 rounded-lg text-sm font-medium transition-colors',
      isActive
        ? 'bg-indigo-600 text-white'
        : 'bg-gray-200 text-gray-800 hover:bg-gray-300 dark:bg-gray-700 dark:text-gray-300 dark:hover:bg-gray-600'
    ]
  }
  
  if (props.variant === 'boxed') {
    return [
      'px-4 py-2 rounded-t-lg text-sm font-medium transition-colors',
      isActive
        ? 'bg-white text-indigo-600 border border-b-0 border-indigo-600 dark:bg-gray-800 dark:text-indigo-400'
        : 'bg-gray-200 text-gray-800 hover:bg-gray-300 dark:bg-gray-900 dark:text-gray-400'
    ]
  }
  
  // underline variant (default)
  return [
    'px-4 py-2 border-b-2 text-sm font-medium transition-colors',
    isActive
      ? 'border-indigo-600 text-indigo-600'
      : 'border-transparent text-gray-700 hover:text-gray-900 hover:border-gray-400 dark:text-gray-400 dark:hover:text-gray-200'
  ]
}

function setActiveTab(slug) {
  emit('update:modelValue', slug)
}

function registerTab(tab) {
  if (!tabs.value.find(t => t.slug === tab.slug)) {
    tabs.value.push(tab)
  }
}

function unregisterTab(slug) {
  const index = tabs.value.findIndex(t => t.slug === slug)
  if (index > -1) {
    tabs.value.splice(index, 1)
  }
}

// Provide context to child Tab components
provide('activeTab', computed(() => props.modelValue))
provide('registerTab', registerTab)
provide('unregisterTab', unregisterTab)
provide('setActiveTab', setActiveTab)
</script>

<template>
  <div class="w-full">
    <!-- Tab Headers -->
    <div :class="tabsContainerClass">
      <button
        v-for="tab in tabs"
        :key="tab.slug"
        :class="getTabButtonClass(tab.slug)"
        @click="setActiveTab(tab.slug)"
      >
        {{ tab.title }}
      </button>
    </div>

    <!-- Tab Content -->
    <div class="mt-4">
      <slot />
    </div>
  </div>
</template>
