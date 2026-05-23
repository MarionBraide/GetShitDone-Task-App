<script setup>
defineProps({
  task: { type: Object, required: true }
})

const emit = defineEmits(['toggle', 'delete'])
</script>

<template>
  <div class="task-row" :class="{ done: task.done }">

    <button
      class="check"
      :class="{ checked: task.done }"
      @click="emit('toggle')"
    >
      <span v-if="task.done">✓</span>
    </button>

    <span class="task-text">{{ task.text }}</span>

    <button class="del" @click="emit('delete')">×</button>

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
.task-row:hover               { border-color: var(--sky-mid); }
.task-row.done                { opacity: 0.45; background: var(--grey-100); border-color: transparent; }
.task-row.done .task-text     { text-decoration: line-through; color: var(--grey-400); }

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
  cursor: pointer;
  font-size: 13px;
  font-weight: 700;
  color: var(--white);
  transition: all 0.15s;
  flex-shrink: 0;
}
.check:hover    { border-color: var(--sky); }
.check.checked  { background: var(--sky); border-color: var(--sky); }

.task-text {
  flex: 1;
  font-size: 15px;
  font-family: var(--font);
  color: var(--black);
}

.del {
  background: transparent;
  border: none;
  color: var(--grey-400);
  font-size: 20px;
  line-height: 1;
  padding: 0 4px;
  cursor: pointer;
  border-radius: 4px;
  transition: color 0.12s;
  flex-shrink: 0;
}
.del:hover { color: #ef4444; }
</style>