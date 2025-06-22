<script setup>
import { ref, computed } from 'vue'
import Card from '../components/Card.vue'

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
  easy: { pairs: 5, cols: 5 },
  medium: { pairs: 7, cols: 7 },
  hard: { pairs: 10, cols: 5 },
}

const gameStatus = ref('selecting_players')
const numberOfPlayers = ref(0)
const turnMessage = ref('')
const mistakes = ref(0)
const state = ref({
  scores: { player1: 0, player2: 0 },
  currentPlayer: 1,
  flippedCards: [],
  matchedPairs: 0,
  totalPairs: 0,
  isLocked: false,
  level: null,
})
const endGameInfo = ref({ title: '', scores: '' })
const gameCards = ref([])

const boardStyle = computed(() => {
  if (!state.value.level) return {}
  const config = LEVEL_CONFIG[state.value.level]
  return {
    gridTemplateColumns: `repeat(${config.cols}, 1fr)`,
  }
})

function setPlayerMode(players) {
  numberOfPlayers.value = players
  gameStatus.value = 'selecting_level'
}

function shuffle(array) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[array[i], array[j]] = [array[j], array[i]]
  }
  return array
}

function startGame(level) {
  state.value.level = level
  state.value.scores = { player1: 0, player2: 0 }
  state.value.currentPlayer = 1
  state.value.flippedCards = []
  state.value.matchedPairs = 0
  state.value.isLocked = true
  mistakes.value = 0

  const config = LEVEL_CONFIG[level]
  state.value.totalPairs = config.pairs

  const selectedCards = ALL_CARDS.slice(0, config.pairs)
  const deck = shuffle([...selectedCards, ...selectedCards])

  gameCards.value = deck.map((card, index) => ({
    ...card,
    id: index,
    isFlipped: false,
    isMatched: false,
  }))

  gameStatus.value = 'playing'
  turnMessage.value = 'The Deal...'

  gameCards.value.forEach((card) => (card.isFlipped = true))

  setTimeout(() => {
    gameCards.value.forEach((card) => (card.isFlipped = false))
    setTimeout(() => {
      state.value.isLocked = false
      updateTurnIndicator()
    }, 700)
  }, 3000)
}

function updateTurnIndicator() {
  if (numberOfPlayers.value === 1) {
    turnMessage.value = `Mistakes: ${mistakes.value}`
  } else {
    turnMessage.value = `Player ${state.value.currentPlayer}'s Turn`
  }
}

function onCardClick(card) {
  if (state.value.isLocked || card.isFlipped || card.isMatched) {
    return
  }

  card.isFlipped = true
  state.value.flippedCards.push(card)

  if (state.value.flippedCards.length === 2) {
    state.value.isLocked = true
    checkForMatch()
  }
}

function checkForMatch() {
  const [card1, card2] = state.value.flippedCards

  setTimeout(() => {
    if (card1.symbol === card2.symbol) {
      handleMatch(card1, card2)
    } else {
      handleMismatch(card1, card2)
    }
  }, 900)
}

function handleMatch(card1, card2) {
  card1.isMatched = true
  card2.isMatched = true
  if (numberOfPlayers.value === 2) {
    state.value.scores[`player${state.value.currentPlayer}`]++
  }
  state.value.matchedPairs++
  state.value.flippedCards = []

  if (state.value.matchedPairs === state.value.totalPairs) {
    endGame()
    return
  }

  state.value.isLocked = false
}

function handleMismatch(card1, card2) {
  card1.isFlipped = false
  card2.isFlipped = false
  if (numberOfPlayers.value === 1) {
    mistakes.value++
  } else {
    state.value.currentPlayer = state.value.currentPlayer === 1 ? 2 : 1
  }
  state.value.flippedCards = []
  state.value.isLocked = false
  updateTurnIndicator()
}

function endGame() {
  if (numberOfPlayers.value === 1) {
    endGameInfo.value.title = 'Round Complete!'
    endGameInfo.value.scores = `You made ${mistakes.value} mistakes.`
  } else {
    const p1Score = state.value.scores.player1
    const p2Score = state.value.scores.player2
    if (p1Score > p2Score) {
      endGameInfo.value.title = 'Player 1 Wins!'
    } else if (p2Score > p1Score) {
      endGameInfo.value.title = 'Player 2 Wins!'
    } else {
      endGameInfo.value.title = "It's a Tie!"
    }
    endGameInfo.value.scores = `Final Score: ${p1Score} - ${p2Score}`
  }
  gameStatus.value = 'ended'
}

function playAgain() {
  gameStatus.value = 'selecting_players'
  state.value.level = null
}
</script>

<template>
  <main class="container">
    <div v-if="gameStatus === 'selecting_players'" class="content-card">
      <h1 class="title">Select Mode</h1>
      <div class="buttons-container">
        <button @click="setPlayerMode(1)" class="level-button low-stakes">1 Player</button>
        <button @click="setPlayerMode(2)" class="level-button high-stakes">2 Players</button>
      </div>
    </div>

    <div v-if="gameStatus === 'selecting_level'" class="content-card">
      <h1 class="title">Select Your Table</h1>
      <div class="buttons-container">
        <button @click="startGame('easy')" class="level-button low-stakes">Low Stakes</button>
        <button @click="startGame('medium')" class="level-button high-stakes">High Stakes</button>
        <button @click="startGame('hard')" class="level-button high-roller">High Roller</button>
      </div>
    </div>

    <div v-else-if="gameStatus === 'playing'" class="game-container">
      <template v-if="numberOfPlayers === 2">
        <div class="player-panel" :class="{ active: state.currentPlayer === 1 }">
          <h2 class="player-title">Player 1</h2>
          <div class="player-score">{{ state.scores.player1 }}</div>
        </div>
      </template>

      <div class="game-board-area" :class="{ 'solo-mode': numberOfPlayers === 1 }">
        <h2 class="turn-indicator">{{ turnMessage }}</h2>
        <div class="game-board" :style="boardStyle">
          <Card
            v-for="card in gameCards"
            :key="card.id"
            :card-data="card"
            @card-clicked="onCardClick"
          />
        </div>
      </div>

      <template v-if="numberOfPlayers === 2">
        <div class="player-panel" :class="{ active: state.currentPlayer === 2 }">
          <h2 class="player-title">Player 2</h2>
          <div class="player-score">{{ state.scores.player2 }}</div>
        </div>
      </template>
    </div>

    <div v-else-if="gameStatus === 'ended'" class="content-card">
      <h1 class="end-game-title">{{ endGameInfo.title }}</h1>
      <p class="final-scores">{{ endGameInfo.scores }}</p>
      <button @click="playAgain" class="level-button high-stakes">New Game</button>
    </div>
  </main>
</template>

<style scoped>
.container {
  flex-grow: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  width: 100%;
}

.content-card {
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

.game-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  gap: 2rem;
  width: 100%;
  max-width: 1200px;
}

.player-panel {
  flex-basis: 20%;
  background-color: rgba(0, 0, 0, 0.4);
  backdrop-filter: blur(8px);
  padding: 1.5rem;
  border-radius: 0.5rem;
  text-align: center;
  transition: all 0.3s ease;
  border: 4px solid transparent;
}

.player-panel.active {
  border-color: #ffd700;
  box-shadow: 0 0 20px #ffd700;
}

.player-title {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
  color: white;
}

.player-score {
  font-size: 3rem;
  font-weight: 700;
  color: #fde047;
}

.game-board-area {
  flex-grow: 1;
}

.game-board-area.solo-mode {
  flex-grow: 0;
}

.turn-indicator {
  font-size: 1.5rem;
  text-align: center;
  font-weight: 700;
  margin-bottom: 1rem;
  letter-spacing: 0.05em;
  min-height: 2.25rem;
  color: white;
}

.game-board {
  display: grid;
  gap: 0.75rem;
  justify-content: center;
  background-color: rgba(0, 0, 0, 0.2);
  padding: 1rem;
  border-radius: 0.5rem;
}

.end-game-title {
  font-size: 3rem;
  font-weight: 700;
  color: #fde047;
  margin-bottom: 1.5rem;
}

.final-scores {
  font-size: 1.5rem;
  margin-bottom: 2rem;
  color: #e5e7eb;
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
