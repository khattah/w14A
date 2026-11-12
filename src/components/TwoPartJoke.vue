
<script setup lang="ts">
import { computed } from 'vue'

type JokeMode = 'normal' | 'loud' | 'snake'

interface Props {
  type: 'single' | 'twopart'
  joke?: string
  setup?: string
  delivery?: string
  mode: JokeMode
}

const props = defineProps<Props>()

function formatJoke(text: string): string {
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
