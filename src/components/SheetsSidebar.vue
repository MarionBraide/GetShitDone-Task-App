<script setup>
// ─────────────────────────────────────────────────────────────
// SheetSidebar.vue — TASK SHEET LIST
//
// Shows all sheets in a sidebar.
// User can click to switch, create a new one, or delete one.
//
// CONCEPTS: defineProps, defineEmits, ref, v-for, v-if,
//           :class, @click, @keyup.enter, v-model
// ─────────────────────────────────────────────────────────────

import { ref } from 'vue'

defineProps({
  sheets:   { type: Array,  required: true },
  activeId: { type: Number, default: null  },
})

const emit = defineEmits(['select', 'add', 'delete'])

// Local state — the new sheet name input
const newName  = ref('')
const creating = ref(false)   // controls whether the input is visible

function submitNew() {
  const name = newName.value.trim()
  if (!name) return
  emit('add', name)
  newName.value  = ''
  creating.value = false
}

function cancelNew() {
  newName.value  = ''
  creating.value = false
}
</script>

<template>
  <aside class="sidebar">

    <div class="sidebar-header">
      <span class="sidebar-title">Sheets</span>
      <!-- + button shows the create input -->
      <button class="new-btn" @click="creating = true" title="New sheet">+</button>
    </div>

    <!-- New sheet input — only visible when creating is true (v-if) -->
    <div v-if="creating" class="new-form">
      <input
        v-model="newName"
        class="new-input"
        placeholder="Sheet name..."
        @keyup.enter="submitNew"
        @keyup.escape="cancelNew"
        autofocus
      />
      <div class="new-actions">
        <button class="action-btn confirm" @click="submitNew">Add</button>
        <button class="action-btn cancel"  @click="cancelNew">Cancel</button>
      </div>
    </div>

    <!-- Sheet list — v-for renders one row per sheet -->
    <nav class="sheet-list">
      <div
        v-for="sheet in sheets"
        :key="sheet.id"
        class="sheet-row"
        :class="{ active: sheet.id === activeId }"
        @click="emit('select', sheet.id)"
      >
        <!-- Sheet name + task count -->
        <div class="sheet-info">
          <span class="sheet-icon">📋</span>
          <div>
            <div class="sheet-name">{{ sheet.name }}</div>
            <div class="sheet-meta">
              {{ sheet.tasks.length }} task{{ sheet.tasks.length !== 1 ? 's' : '' }}
              · @{{ sheet.addedBy }}
            </div>
          </div>
        </div>

        <!-- Delete button — stop propagation so row click doesn't also fire -->
        <button
          class="del-sheet"
          @click.stop="emit('delete', sheet.id)"
          title="Delete sheet"
        >×</button>
      </div>

      <!-- Empty state when no sheets exist -->
      <p v-if="sheets.length === 0" class="no-sheets">No sheets yet. Create one above.</p>
    </nav>

  </aside>
</template>

<style scoped>
.sidebar {
  background: var(--white);
  border: 1.5px solid var(--grey-200);
  border-radius: var(--radius-lg);
  padding: 20px 16px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  align-self: start;
  position: sticky;
  top: 76px;
}

/* ── Header ─────────────────────────────────────────────── */
.sidebar-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.sidebar-title {
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--grey-400);
}

.new-btn {
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: var(--sky);
  border: none;
  color: var(--white);
  font-size: 18px;
  line-height: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.15s;
  font-weight: 300;
}
.new-btn:hover { background: var(--sky-dark); }

/* ── New sheet form ─────────────────────────────────────── */
.new-form {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.new-input {
  width: 100%;
  border: 1.5px solid var(--sky);
  border-radius: var(--radius);
  padding: 8px 12px;
  font-size: 14px;
  color: var(--black);
  background: var(--sky-light);
  outline: none;
}
.new-input::placeholder { color: var(--grey-400); }

.new-actions {
  display: flex;
  gap: 6px;
}

.action-btn {
  flex: 1;
  padding: 6px 0;
  border-radius: var(--radius);
  font-size: 13px;
  font-weight: 600;
  border: none;
  transition: opacity 0.15s;
}
.action-btn.confirm { background: var(--sky); color: var(--white); }
.action-btn.cancel  { background: var(--grey-100); color: var(--grey-600); }
.action-btn:hover   { opacity: 0.8; }

/* ── Sheet list ─────────────────────────────────────────── */
.sheet-list {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.sheet-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 12px;
  border-radius: var(--radius);
  cursor: pointer;
  transition: background 0.12s;
  border: 1.5px solid transparent;
}
.sheet-row:hover  { background: var(--grey-100); }

/* Active sheet gets sky blue border + light blue bg */
.sheet-row.active {
  background: var(--sky-light);
  border-color: var(--sky);
}

.sheet-info {
  display: flex;
  align-items: center;
  gap: 10px;
  min-width: 0;
}

.sheet-icon { font-size: 16px; flex-shrink: 0; }

.sheet-name {
  font-size: 14px;
  font-weight: 600;
  color: var(--black);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.sheet-meta {
  font-size: 11px;
  color: var(--grey-400);
  font-family: var(--font-mono);
  margin-top: 1px;
}

.del-sheet {
  background: transparent;
  border: none;
  color: var(--grey-400);
  font-size: 18px;
  line-height: 1;
  padding: 2px 4px;
  border-radius: 4px;
  flex-shrink: 0;
  transition: color 0.12s;
}
.del-sheet:hover { color: #ef4444; }

.no-sheets {
  font-size: 13px;
  color: var(--grey-400);
  text-align: center;
  padding: 16px 0;
}
</style>
