<script setup>
import { ref } from 'vue'
import { GoogleGenerativeAI } from '@google/generative-ai'

// Receives the full tasks array from App.vue
const props = defineProps({
  tasks: { type: Array, required: true }
})

const summary  = ref('')
const loading  = ref(false)
const error    = ref('')

async function summarise() {
  // Don't call AI if there are no tasks
  if (props.tasks.length === 0) {
    error.value = 'Add some tasks first.'
    return
  }

  loading.value = true
  error.value   = ''
  summary.value = ''

  try {
    const genAI = new GoogleGenerativeAI(import.meta.env.VITE_GEMINI_API_KEY)
    const model = genAI.getGenerativeModel({ model: 'gemini-pro' })

    // Format the task list into a readable string for the prompt
    const taskList = props.tasks
      .map(t => `- ${t.text} [${t.tag || 'no tag'}] (${t.done ? 'done' : 'pending'})`)
      .join('\n')

    const prompt = `
      Here is someone's task list for today:
      ${taskList}

      Write a single short paragraph (2-3 sentences max) summarising
      what they are working on today. Mention how many tasks are pending
      versus done. Keep the tone of the summary casual.
      Do not use bullet points. Just a plain paragraph.
    `

    const result  = await model.generateContent(prompt)
    summary.value = result.response.text()

  } catch (e) {
    error.value = 'Could not reach AI. Check your API key or connection.'
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div class="ai-wrap">

    <div class="ai-header">
      <p class="section-label">AI Summary</p>
      <button
        class="summarise-btn"
        @click="summarise"
        :disabled="loading"
      >
        {{ loading ? 'Thinking...' : '✦ Summarise my day' }}
      </button>
    </div>

    <!-- Error state -->
    <p class="ai-error" v-if="error">{{ error }}</p>

    <!-- Summary output -->
    <div class="ai-output" v-if="summary">
      <p>{{ summary }}</p>
    </div>

  </div>
</template>

<style scoped>
.ai-wrap {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 20px;
  background: var(--grey-100);
  border-radius: var(--radius-lg);
  border: 1.5px solid var(--grey-200);
}

.ai-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.section-label {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--grey-400);
}

.summarise-btn {
  background: var(--black);
  color: var(--white);
  border: none;
  border-radius: var(--radius);
  padding: 10px 18px;
  font-size: 13px;
  font-weight: 700;
  font-family: var(--font);
  cursor: pointer;
  transition: background 0.15s, opacity 0.15s;
  white-space: nowrap;
}
.summarise-btn:hover    { background: var(--sky-dark); }
.summarise-btn:disabled { opacity: 0.5; cursor: not-allowed; }

.ai-output {
  background: var(--white);
  border: 1.5px solid var(--grey-200);
  border-radius: var(--radius);
  padding: 16px;
  font-size: 14px;
  color: var(--black);
  line-height: 1.7;
}

.ai-error {
  font-size: 13px;
  color: #ef4444;
}
</style>