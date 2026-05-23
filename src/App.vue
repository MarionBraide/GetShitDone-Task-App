<script setup>
import { ref } from 'vue'
import AppHeader from './components/AppHeader.vue'
import TaskItem  from './components/TaskItem.vue'

const tasks   = ref([])
const newTask = ref('')
let   nextId  = 1

function addTask() {
  if (!newTask.value.trim()) return
  tasks.value.push({ id: nextId++, text: newTask.value.trim(), done: false })
  newTask.value = ''
}

function toggleTask(id) {
  const t = tasks.value.find(t => t.id === id)
  if (t) t.done = !t.done
}
</script>

<template>
  <div class="layout">
    <AppHeader />

    <main class="main">
      <h2 class="heading">My Tasks</h2>

      <!-- Task list -->
      <div class="list">
        <TaskItem
          v-for="task in tasks"
          :key="task.id"
          :task="task"
          @toggle="toggleTask(task.id)"
        />
      </div>

      <!-- Add task row -->
      <div class="add-row">
        <input
          v-model="newTask"
          class="add-input"
          placeholder="Add a task..."
          @keyup.enter="addTask"
        />
        <button class="add-btn" @click="addTask">Add task</button>
      </div>

    </main>
  </div>
</template>

<style scoped>
.layout {
  min-height: 100vh;
  background: var(--white);
}

.main {
  max-width: 600px;
  margin: 0 auto;
  padding: 48px 24px;
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.heading {
  font-size: 24px;
  font-weight: 800;
  color: var(--black);
  letter-spacing: -0.02em;
}

.list {
  display: flex;
  flex-direction: column;
}

/* Add task row */
.add-row {
  display: flex;
  gap: 10px;
  padding-top: 8px;
}

.add-input {
  flex: 1;
  border: none;
  border-bottom: 2px solid var(--grey-200);
  background: transparent;
  padding: 10px 4px;
  font-size: 15px;
  font-family: var(--font);
  color: var(--black);
  outline: none;
  transition: border-color 0.15s;
}
.add-input:focus        { border-color: var(--sky); }
.add-input::placeholder { color: var(--grey-400); }

.add-btn {
  background: var(--sky);
  color: var(--white);
  border: none;
  border-radius: var(--radius);
  padding: 10px 20px;
  font-size: 14px;
  font-weight: 700;
  font-family: var(--font);
  cursor: pointer;
  transition: background 0.15s;
  white-space: nowrap;
}
.add-btn:hover { background: var(--sky-dark); }
</style>