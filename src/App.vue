
<script setup lang="ts">
import { ref, onMounted } from 'vue'
import axios from 'axios'

import NormalJokeButton from './components/NormalJokeButton.vue'
import JokeDisplay from './components/JokeDisplay.vue'
import JokeSearch from './components/JokeSearch.vue'

type JokeMode = 'normal' | 'loud' | 'snake'

interface Joke {
  type: 'single' | 'twopart'
  joke?: string
  setup?: string
  delivery?: string
}

const joke = ref<Joke | null>(null)
const mode = ref<JokeMode>('normal')
const loading = ref(false)
const error = ref('')

const API_URL = 'https://v2.jokeapi.dev/joke/Any'

async function fetchJoke(keyword = '') {
  loading.value = true
  error.value = ''

  try {
    const response = await axios.get(API_URL, {
      params: {
        safe_mode: '',
        ...(keyword ? { contains: keyword } : {})
      }
    })

    if (response.data.error) {
      throw new Error(
        response.data.message || 'No matching joke found.'
      )
    }

    joke.value = response.data as Joke
    mode.value = 'normal'
  } catch (err) {
    joke.value = null

    if (axios.isAxiosError(err)) {
      error.value =
        err.response?.data?.message ||
        'Unable to connect to JokeAPI.'
    } else if (err instanceof Error) {
      error.value = err.message
    } else {
      error.value = 'Something went wrong.'
    }
  } finally {
    loading.value = false
  }
}

function changeMode(newMode: JokeMode) {
  mode.value = newMode
  // No API call here!
}

onMounted(() => {
  fetchJoke()
})
</script>

<template>
  <main class="app">
    <div class="container">
      <header>
        <h1>😂 Vue Joke Generator</h1>
        <p>Powered by Vue 3, Axios and JokeAPI</p>
      </header>

      <button
        class="generate-button"
        :disabled="loading"
        @click="fetchJoke()"
      >
        {{ loading ? 'Loading...' : 'Generate New Joke' }}
      </button>

      <JokeDisplay
        :joke="joke"
        :mode="mode"
        :loading="loading"
        :error="error"
      />

      <section class="mode-section">
        <h2>Choose Joke Style</h2>

        <div class="mode-buttons">
          <NormalJokeButton
            :disabled="loading || !joke"
            @normal="changeMode('normal')"
          />

          <button
            :disabled="loading || !joke"
            @click="changeMode('loud')"
          >
            🔊 Loud Joke
          </button>

          <button
            :disabled="loading || !joke"
            @click="changeMode('snake')"
          >
            🐍 Snake Joke
          </button>
        </div>
      </section>

      <JokeSearch
        :loading="loading"
        @search="fetchJoke"
      />

      <footer>
        <p>Innotech W14A Assignment</p>
      </footer>
    </div>
  </main>
</template>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f1f5f9;
}

.app {
  min-height: 100vh;
  padding: 40px 20px;
}

.container {
  max-width: 750px;
  margin: auto;
  background: white;
  padding: 30px;
  border-radius: 16px;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.08);
}

header {
  text-align: center;
  margin-bottom: 30px;
}

h1 {
  color: #1e3a8a;
}

header p {
  color: #64748b;
}

.generate-button {
  width: 100%;
  padding: 15px;
  background: #16a34a;
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 18px;
  cursor: pointer;
}

.mode-section {
  margin-top: 25px;
}

.mode-section h2 {
  color: #1e3a8a;
}

.mode-buttons {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.mode-buttons button {
  flex: 1;
  min-width: 130px;
  padding: 12px;
  border: none;
  border-radius: 8px;
  background: #2563eb;
  color: white;
  cursor: pointer;
}

button:hover:not(:disabled) {
  opacity: 0.85;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

footer {
  text-align: center;
  margin-top: 35px;
  color: #64748b;
  font-size: 14px;
}
</style>
