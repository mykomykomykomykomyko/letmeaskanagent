<template>
  <div class="full-wrapper">
  
    <div class="box full-width" id="main-box">
      <h1>Let me ask an agent for you.</h1>
<p>Skip the ping-pong. This tool sends your question directly to an AI agent — no detours, no delay.</p>
      <input v-model="prompt" placeholder="what do you want to know?" @keyup.enter="generateLink" autofocus />
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
import { ref, onMounted } from 'vue'

const prompt = ref('')
const link = ref('')
const submitCount = ref(0)
const copied = ref(false)

const copyToClipboard = () => {
  navigator.clipboard.writeText(link.value)
  copied.value = true
  setTimeout(() => copied.value = false, 2000)
}

const generateLink = async () => {
  submitCount.value++
  if (submitCount.value === 5) {
    alert("🎉 You sure need this tool, don't you?")
  }
  const encoded = encodeURIComponent(prompt.value.trim())
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
  padding: 1rem;
  font-size: 1rem;
  margin-bottom: 1rem;
  border: 1px solid #e0e0e0;
  border-radius: 6px;
  background-color: #fff;
  color: #111;
}

button {
  background-color: #111;
  color: #fff;
  border: none;
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.2s;
  width: 100%;
}

button:hover {
  background-color: #333;
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
.footer-box {
  width: 100%;
  max-width: 720px;
  margin: 2rem auto 0;
  text-align: left;
  background-color: #ffffff;
  padding: 2rem;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

.footer-box h2 {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.footer-box p {
  font-size: 1rem;
  line-height: 1.6;
  color: #333;
}

.footer-box details {
  margin-top: 1rem;
  background-color: #ffffff;
  border: 1px solid #ddd;
  border-radius: 6px;
  padding: 0.75rem 1rem;
  cursor: pointer;
}

.footer-box summary {
  font-weight: 600;
  font-size: 1rem;
  margin-bottom: 0.5rem;
  outline: none;
}

.footer-box details p {
  margin: 0.5rem 0 0;
  font-size: 0.95rem;
  color: #444;
}

.scroll-section {
  margin-bottom: 2rem;
}
.scroll-button {
  margin-top: 1rem;
  background-color: transparent;
  color: #111;
  border: 2px solid #111;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  font-size: 0.95rem;
  cursor: pointer;
  transition: background 0.2s;
  width: 100%;
}

.scroll-button:hover {
  background-color: #111;
  color: #fff;
}
.footer-box ul {
  margin-top: 1rem;
  padding-left: 1rem;
  list-style: disc;
}

.footer-box ul li {
  margin-bottom: 0.75rem;
  font-size: 0.95rem;
  line-height: 1.5;
  color: #333;
}
.content-container { display: none; }

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

</style>
