

<script setup>
import AISummary from './AISummary.vue'
// ─────────────────────────────────────────────────────────────
// TaskBoard.vue — ACTIVE SHEET'S TASK AREA
//
// Shows the selected sheet's name and all its tasks.
// Has an input to add new tasks.
// Emits task events up to App.vue.
//
// CONCEPTS: defineProps, defineEmits, ref, computed,
//           v-if/v-else, v-model, @keyup.enter, scoped styles
// ─────────────────────────────────────────────────────────────

import { ref, computed } from 'vue'
import TaskItem from './TaskItem.vue'

const props = defineProps({
  sheet: { type: Object, default: null }
})

const emit = defineEmits(['add-task', 'toggle-task', 'delete-task'])

const newTask = ref('')

function submit() {
  const text = newTask.value.trim()
  if (!text) return
  emit('add-task', text)
  newTask.value = ''
}

// Computed: split tasks into pending and done for display order
const pendingTasks = computed(() => props.sheet?.tasks.filter(t => !t.done) ?? [])
const doneTasks    = computed(() => props.sheet?.tasks.filter(t =>  t.done) ?? [])
</script>

<template>
  <!-- No sheet selected state -->
  <div v-if="!sheet" class="empty-board">
    <p class="empty-icon">📋</p>
    <p class="empty-msg">Select or create a sheet to get started</p>
  </div>

  <!-- Active sheet view -->
  <div v-else class="board">

    <!-- Sheet heading -->
    <div class="board-header">
      <h1 class="sheet-title">{{ sheet.name }}</h1>
      <span class="sheet-by">by @{{ sheet.addedBy }}</span>
    </div>

    <AISummary :tasks="sheet.tasks" />

    <!-- Add task input -->
    <div class="add-row">
      <input
        v-model="newTask"
        class="add-input"
        placeholder="Add a task and press Enter..."
        @keyup.enter="submit"
      />
      <button class="add-btn" @click="submit">Add</button>
    </div>

    <!-- Pending tasks -->
    <div v-if="pendingTasks.length > 0" class="task-section">
      <p class="section-label">To Do · {{ pendingTasks.length }}</p>
      <div class="task-list">
        <TaskItem
          v-for="task in pendingTasks"
          :key="task.id"
          :task="task"
          @toggle="emit('toggle-task', task.id)"
          @delete="emit('delete-task', task.id)"
        />
      </div>
    </div>

    <!-- Done tasks -->
    <div v-if="doneTasks.length > 0" class="task-section">
      <p class="section-label done-label">Done · {{ doneTasks.length }}</p>
      <div class="task-list">
        <TaskItem
          v-for="task in doneTasks"
          :key="task.id"
          :task="task"
          @toggle="emit('toggle-task', task.id)"
          @delete="emit('delete-task', task.id)"
        />
      </div>
    </div>

    <!-- Truly empty sheet -->
    <div v-if="sheet.tasks.length === 0" class="empty-tasks">
      <p>No tasks yet — add one above ↑</p>
    </div>

  </div>
</template>

<style scoped>
/* ── Empty board ────────────────────────────────────────── */
.empty-board {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 12px;
  background: var(--white);
  border: 1.5px dashed var(--grey-200);
  border-radius: var(--radius-lg);
  padding: 80px 40px;
  text-align: center;
}
.empty-icon { font-size: 40px; }
.empty-msg  { font-size: 15px; color: var(--grey-400); }

/* ── Board ──────────────────────────────────────────────── */
.board {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.board-header {
  display: flex;
  align-items: baseline;
  gap: 12px;
  flex-wrap: wrap;
}

.sheet-title {
  font-size: 28px;
  font-weight: 800;
  color: var(--black);
  letter-spacing: -0.03em;
  line-height: 1.1;
}

.sheet-by {
  font-size: 13px;
  color: var(--grey-400);
  font-family: var(--font-mono);
}

/* ── Add task row ───────────────────────────────────────── */
.add-row {
  display: flex;
  gap: 8px;
}

.add-input {
  flex: 1;
  border: 1.5px solid var(--grey-200);
  border-radius: var(--radius);
  padding: 11px 16px;
  font-size: 15px;
  color: var(--black);
  background: var(--white);
  outline: none;
  transition: border-color 0.15s;
}
.add-input:focus       { border-color: var(--sky); }
.add-input::placeholder { color: var(--grey-400); }

.add-btn {
  background: var(--black);
  color: var(--white);
  border: none;
  border-radius: var(--radius);
  padding: 0 24px;
  font-size: 14px;
  font-weight: 700;
  letter-spacing: 0.02em;
  transition: background 0.15s;
  white-space: nowrap;
}
.add-btn:hover { background: var(--sky-dark); }

/* ── Task sections ──────────────────────────────────────── */
.task-section { display: flex; flex-direction: column; gap: 8px; }

.section-label {
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--grey-400);
}
.done-label { color: var(--sky-dark); }

.task-list { display: flex; flex-direction: column; gap: 6px; }

/* ── Empty task state ───────────────────────────────────── */
.empty-tasks {
  background: var(--white);
  border: 1.5px dashed var(--grey-200);
  border-radius: var(--radius-lg);
  padding: 48px 24px;
  text-align: center;
  font-size: 14px;
  color: var(--grey-400);
}
</style>
