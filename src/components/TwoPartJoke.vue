
<script setup >
import { computed } from 'vue'

const props = defineProps({
  type: { type: String, required: true },
  joke: { type: String, default: '' },
  setup: { type: String, default: '' },
  delivery: { type: String, default: '' },
  mode: { type: String, default: 'normal' }
})

function formatJoke(text) {
  if (props.mode === 'loud') {
    return text.toUpperCase()
  }

  if (props.mode === 'snake') {
    return text.replace(/\s/g, '_')
  }

  return text
}

const formattedJoke = computed(() =>
  formatJoke(props.joke ?? '')
)

const formattedSetup = computed(() =>
  formatJoke(props.setup ?? '')
)

const formattedDelivery = computed(() =>
  formatJoke(props.delivery ?? '')
)
</script>

<template>
  <div class="joke-container">
    <p v-if="type === 'single'" class="joke">
      {{ formattedJoke }}
    </p>

    <div v-else class="two-part">
      <p class="setup">{{ formattedSetup }}</p>
      <p class="delivery">{{ formattedDelivery }}</p>
    </div>
  </div>
</template>

<style scoped>
.joke-container {
  padding: 20px;
  border-radius: 12px;
  background: #f3f4f6;
  color: #222;
}

.joke {
  font-size: 20px;
}

.setup {
  font-size: 20px;
  font-weight: bold;
}

.delivery {
  margin-top: 15px;
  font-size: 20px;
  color: #2563eb;
}
</style>
