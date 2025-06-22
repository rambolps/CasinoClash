<script setup>
import { useRouter } from 'vue-router'

const router = useRouter()

function GoToPlay() {
  router.push('/play')
}
</script>

<template>
  <main class="home-container">
    <div class="content-wrapper">
      <div class="content-card dShadow">
        <h1 class="title">CASINO CLASH</h1>
        <p class="subtitle">
          A two-player memory game of high stakes. Find the most pairs to win the pot!
        </p>
        <button @click="GoToPlay" class="start-button">Place Your Bets</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
.home-container {
  flex-grow: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
}

.content-wrapper {
  width: 100%;
  max-width: 72rem;
  margin-left: auto;
  margin-right: auto;
}

.content-card {
  text-align: center;
  background-color: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(8px);
  padding: 2rem;
  border-radius: 0.5rem;
  border: 2px solid rgba(250, 204, 21, 0.5);
}

.title {
  font-size: 3rem;
  font-weight: 700;
  color: #fde047;
  margin-bottom: 1rem;
  letter-spacing: 0.05em;
}

.subtitle {
  font-size: 1.25rem;
  color: #e5e7eb;
  margin-bottom: 2rem;
}

.start-button {
  background-color: #b91c1c;
  border: 2px solid #ef4444;
  color: white;
  font-weight: 700;
  padding: 0.75rem 2.5rem;
  border-radius: 0.375rem;
  font-size: 1.25rem;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}

.start-button:hover {
  background-color: #991b1b;
  transform: scale(1.05);
}
</style>
