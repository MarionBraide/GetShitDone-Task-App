<script setup>
// ─────────────────────────────────────────────────────────────
//
// Appears as an overlay when user clicks "Add Task".
// Has an input, confirm, and cancel.
// Emits 'add' with the text, or 'close' to dismiss.
//
// CONCEPTS: defineEmits, ref, v-model, @keyup.enter,
//           @keyup.escape, @click.self (click outside to close)
// ─────────────────────────────────────────────────────────────

import { ref, onMounted } from 'vue'

const emit = defineEmits(['add', 'close'])
const text = ref('')
const input = ref(null)

// Auto-focus the input when the modal opens
onMounted(() => input.value?.focus())

function submit() {
  if (!text.value.trim()) return
  emit('add', text.value.trim())
}
</script>

<template>
  <!--
    @click.self — only fires when clicking the backdrop itself,
    not when clicking the modal card inside. This closes on outside click.
  -->
  <div class="backdrop" @click.self="emit('close')" @keyup.escape="emit('close')">

    <div class="modal">

      <div class="modal-header">
        <h2 class="modal-title">New Task</h2>
        <button class="close-btn" @click="emit('close')">
          <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
            <path d="M2 2l12 12M14 2L2 14" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
          </svg>
        </button>
      </div>

      <input
        ref="input"
        v-model="text"
        class="modal-input"
        placeholder="What needs to get done?"
        @keyup.enter="submit"
        @keyup.escape="emit('close')"
        maxlength="120"
      />

      <p class="hint">Press Enter to add · Esc to cancel</p>

      <div class="modal-actions">
        <button class="btn-cancel" @click="emit('close')">Cancel</button>
        <button class="btn-add" @click="submit" :disabled="!text.trim()">Add Task</button>
      </div>

    </div>
  </div>
</template>

<style scoped>
/* ── Backdrop ──────────────────────────────────────────── */
.backdrop {
  position: fixed;
  inset: 0;
  background: rgba(10, 10, 10, 0.45);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 200;
  padding: 24px;
  animation: fadeIn 0.15s ease;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}

/* ── Modal card ────────────────────────────────────────── */
.modal {
  background: var(--white);
  border-radius: var(--radius-lg);
  padding: 28px;
  width: 100%;
  max-width: 480px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  box-shadow: 0 24px 64px rgba(0,0,0,0.18);
  animation: slideUp 0.18s ease;
}

@keyframes slideUp {
  from { opacity: 0; transform: translateY(16px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* ── Header ────────────────────────────────────────────── */
.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.modal-title {
  font-size: 20px;
  font-weight: 800;
  letter-spacing: -0.02em;
  color: var(--black);
}

.close-btn {
  background: var(--grey-100);
  border: none;
  border-radius: 8px;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--grey-400);
  transition: all 0.12s;
}
.close-btn:hover { background: var(--grey-200); color: var(--black); }

/* ── Input ─────────────────────────────────────────────── */
.modal-input {
  width: 100%;
  border: 2px solid var(--grey-200);
  border-radius: var(--radius);
  padding: 14px 16px;
  font-size: 16px;
  font-family: var(--font);
  color: var(--black);
  background: var(--grey-100);
  outline: none;
  transition: border-color 0.15s, background 0.15s;
}
.modal-input:focus       { border-color: var(--sky); background: var(--white); }
.modal-input::placeholder { color: var(--grey-400); }

/* ── Hint ──────────────────────────────────────────────── */
.hint {
  font-size: 12px;
  color: var(--grey-400);
  font-family: var(--font-mono);
  margin-top: -6px;
}

/* ── Actions ───────────────────────────────────────────── */
.modal-actions {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
  margin-top: 4px;
}

.btn-cancel {
  background: var(--grey-100);
  border: none;
  border-radius: var(--radius);
  padding: 11px 20px;
  font-size: 14px;
  font-weight: 600;
  color: var(--grey-600);
  transition: background 0.12s;
}
.btn-cancel:hover { background: var(--grey-200); }

.btn-add {
  background: var(--sky);
  border: none;
  border-radius: var(--radius);
  padding: 11px 24px;
  font-size: 14px;
  font-weight: 700;
  color: var(--white);
  transition: background 0.15s, opacity 0.15s;
}
.btn-add:hover    { background: var(--sky-dark); }
.btn-add:disabled { opacity: 0.4; cursor: not-allowed; }
</style>