<script setup>
// ─────────────────────────────────────────────────────────────
// TaskItem.vue — SINGLE TASK ROW
//
// One task: a checkbox to mark done, the text, and a delete button.
// Never modifies data — emits 'toggle' and 'delete' upward.
//
// CONCEPTS: defineProps, defineEmits, :class, @click, v-if
// ─────────────────────────────────────────────────────────────

defineProps({
  task: { type: Object, required: true }
})

const emit = defineEmits(['toggle', 'delete'])
</script>

<template>
  <!--
    :class="{ done: task.done }" adds the 'done' class when the task
    is marked complete. CSS uses it to style the row differently.
  -->
  <div class="task-row" :class="{ done: task.done }">

    <!-- Custom checkbox — sky blue when checked -->
    <button
      class="check"
      :class="{ checked: task.done }"
      @click="emit('toggle')"
      :aria-label="task.done ? 'Mark incomplete' : 'Mark complete'"
    >
      <span v-if="task.done" class="tick">✓</span>
    </button>

    <!-- Task text — strikethrough applied via CSS when done -->
    <span class="task-text">{{ task.text }}</span>

    <!-- Delete button -->
    <button class="del" @click="emit('delete')" aria-label="Delete task">
      ×
    </button>

  </div>
</template>

<style scoped>
.task-row {
  display: flex;
  align-items: center;
  gap: 12px;
  background: var(--white);
  border: 1.5px solid var(--grey-200);
  border-radius: var(--radius);
  padding: 12px 14px;
  transition: border-color 0.12s, opacity 0.2s;
}
.task-row:hover          { border-color: var(--sky-mid); }

/* Applied when task.done = true */
.task-row.done           { opacity: 0.5; border-color: transparent; background: var(--grey-100); }
.task-row.done .task-text { text-decoration: line-through; color: var(--grey-400); }

/* ── Checkbox ──────────────────────────────────────── */
.check {
  width: 22px;
  min-width: 22px;
  height: 22px;
  border-radius: 6px;
  border: 2px solid var(--grey-200);
  background: var(--white);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.15s;
  flex-shrink: 0;
}
.check:hover          { border-color: var(--sky); }

/* Sky blue fill when task is done */
.check.checked         { background: var(--sky); border-color: var(--sky); }

.tick {
  color: var(--white);
  font-size: 13px;
  font-weight: 700;
  line-height: 1;
}

/* ── Text ──────────────────────────────────────────── */
.task-text {
  flex: 1;
  font-size: 15px;
  color: var(--black);
  line-height: 1.4;
}

/* ── Delete ────────────────────────────────────────── */
.del {
  background: transparent;
  border: none;
  color: var(--grey-400);
  font-size: 20px;
  line-height: 1;
  padding: 0 4px;
  border-radius: 4px;
  transition: color 0.12s;
  flex-shrink: 0;
}
.del:hover { color: #ef4444; }
</style>
