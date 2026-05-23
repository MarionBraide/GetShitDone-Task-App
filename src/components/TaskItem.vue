<script setup>
defineProps({
  task: { type: Object, required: true }
})
const emit = defineEmits(['toggle'])
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
  </div>
</template>

<style scoped>
.task-row {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 16px 4px;
  border-bottom: 1.5px solid var(--grey-200);
  transition: opacity 0.2s;
}
.task-row.done            { opacity: 0.5; }
.task-row.done .task-text { text-decoration: line-through; color: var(--grey-400); }

.check {
  width: 22px;
  min-width: 22px;
  height: 22px;
  border-radius: 4px;
  border: 2px solid var(--grey-200);
  background: var(--white);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 13px;
  font-weight: 700;
  color: var(--white);
  flex-shrink: 0;
  transition: all 0.15s;
}
.check:hover   { border-color: var(--sky); }
.check.checked { background: var(--sky); border-color: var(--sky); }

.task-text {
  flex: 1;
  font-size: 15px;
  font-family: var(--font);
  color: var(--black);
}
</style>