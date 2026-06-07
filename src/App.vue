<script setup>
// ─────────────────────────────────────────────────────────────
// App.vue — ROOT COMPONENT
//
// Owns ALL state:
//   - sheets[]        → array of task sheets
//   - activeSheetId   → which sheet is currently open
//
// Passes data DOWN via props.
// Receives events UP via emits.
//
// CONCEPTS: ref, computed, watch, props, emits
// ─────────────────────────────────────────────────────────────

import { ref, computed, watch } from 'vue'
import { initialSheets } from './data/tasks.js'

import AppHeader   from './components/AppHeader.vue'
import SheetSidebar from './components/SheetSidebar.vue'
import TaskBoard   from './components/TaskBoard.vue'

// ── State ──────────────────────────────────────────────────
const saved = localStorage.getItem('gsd-sheets')
const sheets = ref(saved ? JSON.parse(saved) : JSON.parse(JSON.stringify(initialSheets)))
const activeSheetId = ref(sheets.value[0]?.id ?? null)

let nextSheetId = sheets.value.length
  ? Math.max(...sheets.value.map(s => s.id)) + 1
  : 1

// ── Computed ───────────────────────────────────────────────
const activeSheet = computed(() =>
  sheets.value.find(s => s.id === activeSheetId.value) ?? null
)

// ── Watcher — auto save ────────────────────────────────────
watch(sheets, val => localStorage.setItem('gsd-sheets', JSON.stringify(val)), { deep: true })

// ── Sheet methods ──────────────────────────────────────────
function addSheet(name) {
  const sheet = { id: nextSheetId++, name, addedBy: 'me', tasks: [] }
  sheets.value.push(sheet)
  activeSheetId.value = sheet.id
}

function deleteSheet(id) {
  sheets.value = sheets.value.filter(s => s.id !== id)
  // If we deleted the active sheet, switch to first remaining
  if (activeSheetId.value === id) {
    activeSheetId.value = sheets.value[0]?.id ?? null
  }
}

// ── Task methods ───────────────────────────────────────────
function addTask(text) {
  if (!activeSheet.value) return
  const tasks = activeSheet.value.tasks
  const nextTaskId = tasks.length ? Math.max(...tasks.map(t => t.id)) + 1 : 1
  tasks.push({ id: nextTaskId, text, done: false })
}

function toggleTask(taskId) {
  const task = activeSheet.value?.tasks.find(t => t.id === taskId)
  if (task) task.done = !task.done
}

function deleteTask(taskId) {
  if (!activeSheet.value) return
  activeSheet.value.tasks = activeSheet.value.tasks.filter(t => t.id !== taskId)
}
</script>

<template>
  <div class="layout">

    <AppHeader />

    <div class="body">

      <!-- Sidebar: sheet list + create new sheet -->
      <SheetSidebar
        :sheets="sheets"
        :active-id="activeSheetId"
        @select="activeSheetId = $event"
        @add="addSheet"
        @delete="deleteSheet"
      />

      <!-- Main board: tasks for the active sheet -->
      <TaskBoard
        :sheet="activeSheet"
        @add-task="addTask"
        @toggle-task="toggleTask"
        @delete-task="deleteTask"
      />

    </div>
  </div>
</template>

<style scoped>
.layout {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.body {
  flex: 1;
  display: grid;
  grid-template-columns: 260px 1fr;
  max-width: 1100px;
  width: 100%;
  margin: 0 auto;
  padding: 32px 24px 48px;
  gap: 24px;
}

@media (max-width: 680px) {
  .body {
    grid-template-columns: 1fr;
    padding: 16px;
  }
}
</style>
