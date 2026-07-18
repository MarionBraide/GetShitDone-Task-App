<script setup>
import { ref } from 'vue'
import Groq from 'groq-sdk'

const props = defineProps({
  tasks: { type: Array, required: true }
})

const summary = ref('')
const loading = ref(false)
const error   = ref('')

async function summarise() {
  if (props.tasks.length === 0) {
    error.value = 'Add some tasks first.'
    return
  }

  loading.value = true
  error.value   = ''
  summary.value = ''

  try {
    const groq = new Groq({
      apiKey: import.meta.env.VITE_GROQ_API_KEY,
      dangerouslyAllowBrowser: true  // required for client-side calls
    })

    const taskList = props.tasks
      .map(t => `- ${t.text} (${t.done ? 'done' : 'pending'})`)
      .join('\n')

    const response = await groq.chat.completions.create({
      model: 'llama-3.3-70b-versatile',
      messages: [
        {
          role: 'user',
          content: `
            Here is someone's task list:
            ${taskList}

            Write a single short paragraph (2-3 sentences max) summarising
            what they are working on today. The first sentence should mention how many tasks are pending
            versus done. The next sentence should highlight what task is priority based on the date and time
            it was created(older tasks get more priority). Keep the tone casual and encouraging.
            Plain paragraph only, no bullet points.
          `
        }
      ],
      max_tokens: 150
    })

    summary.value = response.choices[0].message.content

  } catch (e) {
    console.error('AI error:', e.message)
    if (e.message?.includes('429')) {
      error.value = 'Too many requests. Wait a moment and try again.'
    } else {
      error.value = 'Could not reach AI. Check your API key or connection.'
    }
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div class="ai-wrap">

    <div class="ai-row">
      <span class="ai-label">AI SUMMARY</span>
      <button
        class="summarise-btn"
        @click="summarise"
        :disabled="loading"
      >
        {{ loading ? 'Thinking...' : '✦ Summarise my day' }}
      </button>
    </div>

    <!-- Error -->
    <p class="ai-error" v-if="error">{{ error }}</p>

    <!-- Output -->
    <div class="ai-output" v-if="summary">
      <p>{{ summary }}</p>
    </div>

  </div>
</template>

<style scoped>
.ai-wrap {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 14px 16px;
  background: var(--white);
  border-radius: var(--radius-lg);
  border: 1.5px solid var(--grey-200);
  margin-top: 12px;
}

.ai-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
}

.ai-label {
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.12em;
  color: var(--grey-400);
}

.summarise-btn {
  background: var(--black);
  color: var(--white);
  border: none;
  border-radius: var(--radius);
  padding: 8px 14px;
  font-size: 12px;
  font-weight: 700;
  font-family: var(--font);
  cursor: pointer;
  transition: background 0.15s, opacity 0.15s;
  white-space: nowrap;
}
.summarise-btn:hover    { background: var(--sky-dark); }
.summarise-btn:disabled { opacity: 0.5; cursor: not-allowed; }

.ai-output {
  background: var(--grey-100);
  border-radius: var(--radius);
  padding: 12px 14px;
  font-size: 13px;
  color: var(--black);
  line-height: 1.7;
}

.ai-error {
  font-size: 12px;
  color: #ef4444;
}
</style>