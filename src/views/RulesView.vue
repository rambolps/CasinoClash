<script setup></script>

<template>
  <main class="rules-container">
    <div class="rules-card">
      <h1 class="main-title">Game Rules</h1>

      <section class="rule-section">
        <h2 class="section-title">Objective</h2>
        <p>
          The goal of Casino Clash is to find all the matching pairs of cards on the board. How you
          win depends on the game mode you choose.
        </p>
      </section>

      <section class="rule-section">
        <h2 class="section-title">How to Play</h2>
        <p>
          At the start of each round, all cards will be briefly shown face-up. Memorize their
          positions! After this "deal," the cards will flip face-down and the game will begin.
        </p>
        <p>Click on any two cards to flip them over and reveal their symbols.</p>
      </section>

      <section class="rule-section">
        <h2 class="section-title">Game Modes</h2>
        <div class="mode-details">
          <h3>1 Player Mode</h3>
          <ul>
            <li>Your goal is to clear the board by finding all the pairs.</li>
            <li>If you flip two cards that do not match, it counts as one mistake.</li>
            <li>The game ends when all pairs are found. Try to finish with the fewest mistakes!</li>
          </ul>
        </div>
        <div class="mode-details">
          <h3>2 Player Mode</h3>
          <ul>
            <li>Players take turns flipping two cards.</li>
            <li>
              If the cards are a match, that player scores a point and gets to take another turn.
            </li>
            <li>If the cards do not match, the turn ends and passes to the other player.</li>
            <li>
              The game ends when all pairs are found. The player with the most points wins the pot!
            </li>
          </ul>
        </div>
      </section>
    </div>
  </main>
</template>

<style scoped>
.rules-container {
  flex-grow: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem 1rem;
}

.rules-card {
  width: 100%;
  max-width: 800px;
  text-align: left;
  background-color: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(8px);
  padding: 2rem 3rem;
  border-radius: 0.5rem;
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
  border: 2px solid rgba(250, 204, 21, 0.5);
  color: #e5e7eb;
}

.main-title {
  font-size: 3rem;
  font-weight: 700;
  color: #facc15;
  text-align: center;
  margin-bottom: 2rem;
}

.section-title {
  font-size: 1.75rem;
  font-weight: 700;
  color: #ffffff;
  margin-top: 1.5rem;
  margin-bottom: 1rem;
  border-bottom: 2px solid rgba(250, 204, 21, 0.5);
  padding-bottom: 0.5rem;
}

.rule-section p {
  font-size: 1.1rem;
  line-height: 1.6;
  margin-bottom: 1rem;
}

.mode-details {
  margin-top: 1rem;
}

.mode-details h3 {
  font-size: 1.25rem;
  font-weight: 600;
  color: white;
  margin-bottom: 0.5rem;
}

.mode-details ul {
  list-style-position: inside;
  padding-left: 0.5rem;
}

.mode-details li {
  font-size: 1.1rem;
  line-height: 1.6;
  margin-bottom: 0.5rem;
}
</style>
