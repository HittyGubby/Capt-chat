<script setup>
import { ref } from 'vue'

const messages = ref([])
const input = ref('')
const loading = ref(false)

async function sendMessage() {
  if (!input.value.trim()) return
  messages.value.push({ from: 'You', text: input.value })
  loading.value = true
  try {
    const res = await fetch(`https://proxi.hittygubby.workers.dev?url=${encodeURIComponent('https://crossorigin.me/https://neal.fun/api/not-a-robot/reverse-turing')}`, {
      credentials: 'include',
      headers: {
        'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0',
        'Accept': '*/*',
        'Accept-Language': 'en-US,en;q=0.5',
        'Content-Type': 'text/plain;charset=UTF-8',
        'Priority': 'u=4',
        'Pragma': 'no-cache',
        'Cache-Control': 'no-cache',
        'Sec-Fetch-Dest': 'empty',
        'Sec-Fetch-Mode': 'cors',
        'Sec-Fetch-Site': 'same-origin'
      },
      referrer: 'https://neal.fun/not-a-robot/',
      body: JSON.stringify({ messages: [{ from: 'You', text: input.value }] }),
      method: 'POST',
      mode: 'cors'
    })
    const data = await res.json()
    messages.value.push({ from: 'Bot', text: data.response })
  } catch (e) {
    messages.value.push({ from: 'Bot', text: 'Error' })
  }
  loading.value = false
  input.value = ''
  scrollToBottom()
}

function handleKeydown(e) {
  if (e.key === 'Enter' && !e.shiftKey) {
    e.preventDefault()
    sendMessage()
  }
}

function scrollToBottom() {
  nextTick(() => {
    const chat = document.querySelector('.chat')
    chat.scrollTop = chat.scrollHeight
  })
}
</script>

<template>
  <div class="container">
    <div class="chat">
      <div v-for="(msg, i) in messages" :key="i" :class="['bubble', msg.from === 'You' ? 'user' : 'bot']">
        <div class="text">{{ msg.text }}</div>
      </div>
      <div v-if="loading" class="bubble bot loading">Thinking...</div>
    </div>
    <div class="input">
      <textarea v-model="input" @keydown="handleKeydown" placeholder="Type here..." rows="1"></textarea>
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<style>
body {
  background: #121212;
  color: #fff;
  font-family: Arial;
  margin: 0;
}

.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
}

.chat {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
}

.bubble {
  max-width: 70%;
  margin-bottom: 10px;
  padding: 10px 15px;
  border-radius: 20px;
  animation: fadeIn 0.3s;
}

.user {
  background: #1e88e5;
  align-self: flex-end;
  margin-left: auto;
}

.bot {
  background: #424242;
  align-self: flex-start;
}

.text {
  white-space: pre-wrap;
}

.loading {
  animation: pulse 1s infinite;
}

.input {
  display: flex;
  padding: 10px;
  background: #212121;
}

textarea {
  flex: 1;
  background: #333;
  color: #fff;
  border: none;
  padding: 10px;
  border-radius: 10px;
  resize: none;
}

button {
  background: #1e88e5;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin-left: 10px;
  border-radius: 10px;
  cursor: pointer;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes pulse {
  0% {
    opacity: 0.5;
  }

  50% {
    opacity: 1;
  }

  100% {
    opacity: 0.5;
  }
}
</style>