<template>
  <button :class="btnClass" @click="onClick">
    <slot></slot>
  </button>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  type: { type: String, default: 'default' },
  extra: { type: String, default: '' }
})

const emit = defineEmits(['click'])

const btnClass = computed(() => {
  let base = 'text-xl rounded h-12 flex items-center justify-center select-none'
  if (props.type === 'operator') {
    base += ' bg-yellow-500 text-black'
  } else if (props.type === 'action') {
    base += ' bg-gray-700 text-white'
  } else {
    base += ' bg-gray-800 text-white'
  }
  return `${base} ${props.extra}`
})

function onClick() {
  emit('click')
}
</script>



