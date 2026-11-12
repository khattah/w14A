
<script setup lang="ts">
import TwoPartJoke from './TwoPartJoke.vue'

type JokeMode = 'normal' | 'loud' | 'snake'

interface Joke {
  type: 'single' | 'twopart'
  joke?: string
  setup?: string
  delivery?: string
}

defineProps<{
  joke: Joke | null
  mode: JokeMode
  loading: boolean
  error: string
}>()
</script>

<template>
  <section class="joke-display">
    <h2>Your Joke</h2>

    <p v-if="loading" class="loading">
      Loading joke...
    </p>

    <p v-else-if="error" class="error">
      {{ error }}
    </p>

    <p v-else-if="!joke" class="empty">
      Click "Generate New Joke" to get started!
    </p>

    <TwoPartJoke
      v-else
      :type="joke.type"
      :joke="joke.joke"
      :setup="joke.setup"
      :delivery="joke.delivery"
      :mode="mode"
    />
  </section>
</template>

<style scoped>
.joke-display {
  margin: 20px 0;
  padding: 20px;
  background: white;
  border-radius: 12px;
  border: 1px solid #ddd;
}

h2 {
  margin-bottom: 15px;
  color: #1e3a8a;
}

.loading {
  color: #2563eb;
}

.error {
  color: #dc2626;
}

.empty {
  color: #6b7280;
  font-style: italic;
}
</style>
