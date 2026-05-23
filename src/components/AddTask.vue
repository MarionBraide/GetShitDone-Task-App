<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add-task'])
const text = ref('')

function submit() {
  if (!text.value.trim()) return
  emit('add-task', text.value.trim())
  text.value = ''
}
</script>

<template>
  <div class="input-row">
    <input
      v-model="text"
      class="input"
      placeholder="Add a task..."
      @keyup.enter="submit"
    />
    <button class="btn" @click="submit">Add</button>
  </div>
</template>

<style scoped>
.input-row {
  display: flex;
  gap: 8px;
}

.input {
  flex: 1;
  border: 1.5px solid var(--grey-200);
  border-radius: var(--radius);
  padding: 11px 16px;
  font-size: 15px;
  font-family: var(--font);
  color: var(--black);
  background: var(--white);
  outline: none;
  transition: border-color 0.15s;
}
.input:focus        { border-color: var(--sky); }
.input::placeholder { color: var(--grey-400); }

.btn {
  background: var(--black);
  color: var(--white);
  border: none;
  border-radius: var(--radius);
  padding: 0 24px;
  font-family: var(--font);
  font-size: 14px;
  font-weight: 700;
  cursor: pointer;
  transition: background 0.15s;
  white-space: nowrap;
}
.btn:hover { background: var(--sky-dark); }
</style>