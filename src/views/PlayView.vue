<script setup>
import { ref } from 'vue'

const ALL_CARDS = [
  { symbol: '♥', color: 'red' },
  { symbol: '♦', color: 'red' },
  { symbol: '♣', color: 'black' },
  { symbol: '♠', color: 'black' },
  { symbol: 'K', color: 'red' },
  { symbol: 'Q', color: 'black' },
  { symbol: 'J', color: 'red' },
  { symbol: 'A', color: 'black' },
  { symbol: '10', color: 'red' },
  { symbol: '9', color: 'black' },
]
const LEVEL_CONFIG = {
  easy: { pairs: 5, cols: 5, rows: 2 },
  medium: { pairs: 7, cols: 7, rows: 2 },
  hard: { pairs: 10, cols: 5, rows: 4 },
}
let state = ref({
  level: 'null',
  scores: { player1: 0, player2: 0 },
  currentPlayer: 1,
  flippedCards: [],
  matchedPairs: 0,
  totalPairs: 0,
  isLocked: false,
})

const selectLevel = (level) => {
  state.value.level = level
}
</script>

<template>
  <main class="container">
    <div v-if="state.level == 'null'" class="level-select-card">
      <h1 class="title">Select Your Table</h1>
      <div class="buttons-container">
        <button @click="selectLevel('easy')" class="level-button low-stakes">Low Stakes</button>
        <button @click="selectLevel('medium')" class="level-button high-stakes">High Stakes</button>
        <button @click="selectLevel('hard')" class="level-button high-roller">High Roller</button>
      </div>
    </div>

    <div v-if="state.level == 'easy'" class="level-select-card"></div>
  </main>
</template>

<style scoped>
.container {
  flex-grow: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
}

.level-select-card {
  text-align: center;
  background-color: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(8px);
  padding: 2rem;
  border-radius: 0.5rem;
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
  border: 2px solid rgba(250, 204, 21, 0.5);
}

.title {
  font-size: 2.25rem;
  font-weight: 700;
  color: #fde047;
  margin-bottom: 2rem;
}

.buttons-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 1rem;
}

.level-button {
  width: 100%;
  color: white;
  border: none;
  cursor: pointer;
  font-weight: 700;
  padding: 1rem 2.5rem;
  border-radius: 0.375rem;
  font-size: 1.25rem;
  box-shadow:
    0 10px 15px -3px rgba(0, 0, 0, 0.1),
    0 4px 6px -2px rgba(0, 0, 0, 0.05);
  transition: all 0.2s ease-in-out;
}

.level-button:hover {
  transform: scale(1.05);
}

.low-stakes {
  background-color: #4b5563;
}
.low-stakes:hover {
  background-color: #374151;
}

.high-stakes {
  background-color: #b91c1c;
}
.high-stakes:hover {
  background-color: #991b1b;
}

.high-roller {
  background-color: #6b21a8;
}
.high-roller:hover {
  background-color: #581c87;
}

@media (min-width: 768px) {
  .buttons-container {
    flex-direction: row;
    gap: 1.5rem;
  }
  .level-button {
    width: auto;
  }
}
</style>
