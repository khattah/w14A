
<script setup >
import { ref } from 'vue'

defineProps({
  loading: Boolean
})

const emit = defineEmits(['search'])

const keyword = ref('')

function searchJoke() {
  const searchText = keyword.value.trim()

  if (!searchText) {
    alert('Please enter a word to search.')
    return
  }

  emit('search', searchText)
}
</script>

<template>
  <div class="search-container">
    <h2>Search for a Joke</h2>

    <form @submit.prevent="searchJoke">
      <input
        v-model="keyword"
        type="text"
        placeholder="Enter a word, e.g. computer"
        :disabled="loading"
      />

      <button
        type="submit"
        :disabled="loading"
      >
        {{ loading ? 'Searching...' : 'Search Joke' }}
      </button>
    </form>
  </div>
</template>

<style scoped>
.search-container {
  margin-top: 25px;
  padding: 20px;
  background: #e0f2fe;
  border-radius: 12px;
}

h2 {
  margin-bottom: 15px;
  color: #1e3a8a;
}

form {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

input {
  flex: 1;
  min-width: 180px;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 16px;
}

button {
  padding: 12px 20px;
  background: #2563eb;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>
