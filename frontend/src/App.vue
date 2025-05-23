<template>
  <div class="full-wrapper">
    <div class="box full-width" id="main-box">
      <h1>Let me ask an agent for you.</h1>
      <p>Skip the ping-pong. This tool sends your question directly to an AI agent — no detours, no delay.</p>
      <input
        v-model="prompt"
        :class="{ 'error-input': showError }"
        placeholder="what do you want to know?"
        @keyup.enter="generateLink"
        autofocus
      />
      <button @click="generateLink">Submit your question</button>

      <div v-if="link" class="result success">
        <h2>✨ Congratulations! Here's your answer.</h2>
        <a :href="link" target="_blank">{{ link }}</a>
        <p>This link will open your GoA's secure ChatGPT where your questions will be answered.</p>
        <button class="copy-btn" @click="copyToClipboard">📋 Copy to clipboard and share with your colleague</button>
      </div>
      <span v-if="copied" class="toast">✅ Copied!</span>
    </div>
    <footer class="footer-credit">built by <a href="https://www.linkedin.com/in/mykodev/" target="_blank" rel="noopener noreferrer">Mykola</a> at 12:15AM</footer>
  </div>
</template>

<script setup>
import confetti from 'canvas-confetti'
import { ref } from 'vue'

const prompt = ref('')
const link = ref('')
const submitCount = ref(0)
const copied = ref(false)
const showError = ref(false)

const copyToClipboard = () => {
  navigator.clipboard.writeText(link.value)
  copied.value = true
  setTimeout(() => copied.value = false, 2000)
}

const generateLink = async () => {
  const trimmed = prompt.value.trim()
  if (!trimmed) {
    showError.value = true
    setTimeout(() => showError.value = false, 1500)
    return
  }

  submitCount.value++
  if (submitCount.value === 5) {
    alert("🎉 You sure need this tool, don't you?")
  }

  const encoded = encodeURIComponent(trimmed)
  link.value = `https://chat.openai.com/?model=gpt-4&prompt=${encoded}`
  scrollToResult()
  prompt.value = ''
  confetti({
    particleCount: 80,
    spread: 90,
    origin: { y: 0.6 },
    ticks: 300
  })
}

const scrollToResult = () => {
  setTimeout(() => {
    document.querySelector('.result')?.scrollIntoView({ behavior: 'smooth' })
  }, 50)
}
</script>

<style scoped>
html, body {
  margin: 0;
  padding: 0;
  overflow-x: hidden;
  overflow-y: auto;
  height: 100%;
}

.full-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  background: linear-gradient(270deg, #ffffff, #f0f0f0, #d1d1d1, #f6d365, #ffffff);
  background-size: 2500% 2500%;
  animation: gradientShift 80s ease infinite;
  font-family: -apple-system, BlinkMacSystemFont, "San Francisco", "Helvetica Neue", Helvetica, Arial, sans-serif;
  color: #111;
  padding: 2rem 1rem;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
}

@keyframes gradientShift {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.box {
  margin: 2rem auto;
  background-color: #ffffff;
  padding: 2rem;
  border-radius: 12px;
  width: 100%;
  max-width: 600px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
  text-align: left;
}

h1 {
  font-size: 1.6rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
}

p {
  font-size: 1rem;
  margin-bottom: 1.5rem;
}

input {
  width: 100%;
  max-width: 600px;
  padding: 1rem;
  font-size: 1rem;
  margin-bottom: 1.25rem;
  border: 2px solid #111;
  border-radius: 8px;
  background-color: #fff;
  color: #111;
  box-sizing: border-box;
}

.error-input {
  border-color: red !important;
  animation: shake 0.3s ease-in-out;
}

@keyframes shake {
  0% { transform: translateX(0); }
  20% { transform: translateX(-4px); }
  40% { transform: translateX(4px); }
  60% { transform: translateX(-4px); }
  80% { transform: translateX(4px); }
  100% { transform: translateX(0); }
}

button {
  display: block;
  margin: 1rem auto;
  padding: 0.9rem 1.5rem;
  font-size: 1rem;
  font-weight: 600;
  text-align: center;
  color: #fff;
  background-color: #111;
  border: none;
  border-radius: 8px;
  width: 100%;
  max-width: 600px;
  transition: background-color 0.2s ease, transform 0.1s ease;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

button:hover {
  background-color: #333;
  transform: scale(1.01);
}

.result {
  margin-top: 1.5rem;
  font-size: 0.95rem;
  word-break: break-word;
}

a {
  color: #000;
  text-decoration: underline;
}

a:hover {
  opacity: 0.7;
}

.footer-credit {
  position: absolute;
  bottom: 1rem;
  width: 100%;
  text-align: center;
  font-size: 0.85rem;
  color: #666;
  font-style: italic;
}

.footer-credit a {
  color: #666;
  text-decoration: underline;
}

.footer-credit a:hover {
  opacity: 0.7;
}

.copy-btn {
  margin-left: 0.75rem;
  background-color: #111;
  color: #fff;
  border: none;
  padding: 0.4rem 0.75rem;
  font-size: 0.9rem;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s;
}

.copy-btn:hover {
  background-color: #333;
}

.result.success {
  background: #f8f8f8;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 1.5rem;
  margin-top: 2rem;
  text-align: center;
}

.result.success h2 {
  font-size: 1.2rem;
  margin-bottom: 1rem;
}

.result.success a {
  display: block;
  font-size: 0.95rem;
  margin-bottom: 0.75rem;
}

.toast {
  display: inline-block;
  margin-top: 1rem;
  color: green;
  font-size: 0.95rem;
  font-weight: 500;
  animation: fadeInOut 2s ease;
}

@keyframes fadeInOut {
  0% { opacity: 0; }
  20% { opacity: 1; }
  80% { opacity: 1; }
  100% { opacity: 0; }
}
</style>
