
<script setup >
defineProps({
  disabled: Boolean
})

const emit = defineEmits(['normal'])
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
