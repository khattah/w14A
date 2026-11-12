
<script setup lang="ts">
defineProps<{
  disabled?: boolean
}>()

const emit = defineEmits<{
  (e: 'normal'): void
}>()
</script>

<template>
  <button
    type="button"
    :disabled="disabled"
    @click="emit('normal')"
  >
    Normal Joke
  </button>
</template>
