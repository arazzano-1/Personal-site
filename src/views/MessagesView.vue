<script setup lang="ts">
import { onMounted, ref } from 'vue'
import { supabaseClient } from '../supabase'

const inputText = ref('')
const messages = ref<string[]>([])
const isSending = ref(false)
const isLoading = ref(false)
const errorMessage = ref('')

async function sendMessage() {
  const trimmed = inputText.value.trim()
  if (!trimmed || isSending.value) {
    return
  }

  isSending.value = true
  errorMessage.value = ''

  const { error } = await supabaseClient
    .from('siteMessages')
    .insert({ message: trimmed });

  if (error) {
    console.error('Error sending message:', error)
    errorMessage.value = 'Failed to send message. Please try again.'
  } 
  else {
    inputText.value = ''
    await fetchMessages()
  }

  isSending.value = false
}

async function fetchMessages() {
  if (isLoading.value) {
    return
  }

  isLoading.value = true
  errorMessage.value = ''

  const { data, error } = await supabaseClient
    .from('siteMessages')
    .select('message')
    .order('id', { ascending: false })

  if (error) {
    console.error('Error fetching messages:', error)
    messages.value = []
    errorMessage.value = 'Failed to load messages.'
  } else {
    messages.value = data.map((item: { message: string }) => item.message)
  }

  isLoading.value = false
}

onMounted(() => {
  fetchMessages()
})
</script>

<template>
  <section class="messages-page">
    <header class="page-header">
      <h1>Messages</h1>
      <button class="secondary" @click="fetchMessages" :disabled="isLoading">
        {{ isLoading ? 'Loading...' : 'Refresh' }}
      </button>
    </header>

    <div class="message-form">
      <label class="input-label" for="message-input">New message</label>
      <textarea
        id="message-input"
        v-model="inputText"
        rows="3"
        placeholder="Write a message..."
      ></textarea>
      <div class="actions">
        <button @click="sendMessage" :disabled="isSending || !inputText.trim()">
          {{ isSending ? 'Sending...' : 'Send message' }}
        </button>
      </div>
      <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
    </div>

    <div class="message-board">
      <h2>Latest messages</h2>
      <p v-if="!messages.length && !isLoading" class="empty-state">
        No messages yet. Be the first to leave one.
      </p>
      <ul v-else>
        <li v-for="(msg, idx) in messages" :key="idx">{{ msg }}</li>
      </ul>
    </div>
  </section>
</template>

<style scoped>
.messages-page {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.page-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
}

.message-form {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 16px;
  border-radius: 12px;
  border: 1px solid var(--color-border);
  background: var(--color-background-soft);
}

.input-label {
  font-weight: 600;
}

textarea {
  width: 100%;
  border-radius: 8px;
  border: 1px solid var(--color-border);
  padding: 10px 12px;
  font-family: inherit;
  font-size: 1rem;
}

.actions {
  display: flex;
  gap: 12px;
}

button {
  cursor: pointer;
  border-radius: 8px;
  border: 1px solid transparent;
  padding: 8px 14px;
  font-size: 0.95rem;
  background: var(--color-heading);
  color: var(--color-background);
}

button.secondary {
  background: transparent;
  color: inherit;
  border-color: var(--color-border);
}

button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.message-board {
  padding: 16px;
  border-radius: 12px;
  border: 1px solid var(--color-border);
  background: var(--color-background-mute);
}

.message-board ul {
  list-style: none;
  padding-left: 0;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.message-board li {
  padding: 10px 12px;
  border-radius: 8px;
  background: var(--color-background);
  border: 1px solid var(--color-border);
}

.empty-state {
  color: var(--color-text);
  opacity: 0.75;
}

.error {
  color: #b42318;
}
</style>
