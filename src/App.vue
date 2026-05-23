<script setup>
import { ref } from 'vue'
import AppHeader from './components/AppHeader.vue'
import AddTask from './components/AddTask.vue'
import TaskItem  from './components/TaskItem.vue'

const tasks = ref([])
let nextId = 1

function addTask(text) {
  tasks.value.push({ id: nextId++, text, done: false })
}

function toggleTask(id) {
  const t = tasks.value.find(t => t.id === id)
  if (t) t.done = !t.done
}

function deleteTask(id) {
  tasks.value = tasks.value.filter(t => t.id !== id)
}
</script>

<template>
  <div class="layout">
    <AppHeader />

    <main class="main">
      <h2 class="heading">My Tasks</h2>

      <TaskInput @add-task="addTask" />

      <div class="list">
        <TaskItem
          v-for="task in tasks"
          :key="task.id"
          :task="task"
          @toggle="toggleTask(task.id)"
          @delete="deleteTask(task.id)"
        />
        <p v-if="tasks.length === 0" class="empty">No tasks yet. Add one above ↑</p>
      </div>
    </main>
  </div>
</template>

<style scoped>
.layout {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.main {
  max-width: 640px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 24px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.heading {
  font-size: 22px;
  font-weight: 800;
  letter-spacing: -0.02em;
  color: var(--black);
}

.list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.empty {
  text-align: center;
  padding: 48px 0;
  font-size: 14px;
  color: var(--grey-400);
}
</style>