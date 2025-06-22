<script setup>
defineProps({
  cardData: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['card-clicked'])
</script>

<template>
  <div
    class="card"
    :class="{ flipped: cardData.isFlipped, matched: cardData.isMatched }"
    @click="emit('card-clicked', cardData)"
  >
    <div class="card-inner">
      <div class="card-face card-front"></div>
      <div class="card-face card-back">
        <span
          :class="{
            'card-symbol-red': cardData.color === 'red',
            'card-symbol-black': cardData.color === 'black',
          }"
        >
          {{ cardData.symbol }}
        </span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  width: 100px;
  height: 140px;
  perspective: 1000px;
  cursor: pointer;
  user-select: none;
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transition: transform 0.7s;
  transform-style: preserve-3d;
}

.card.flipped .card-inner {
  transform: rotateY(180deg);
}

.card.matched {
  transform: scale(0.95);
  opacity: 0.5;
  cursor: default;
}

.card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 0.5rem;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
}

.card-front {
  background-color: #b71c1c;
  border: 2px solid #d32f2f;
  background-image: repeating-linear-gradient(
    45deg,
    rgba(255, 255, 255, 0.05),
    rgba(255, 255, 255, 0.05) 10px,
    transparent 10px,
    transparent 20px
  );
}

.card-back {
  background-color: #fff;
  border: 2px solid #ddd;
  transform: rotateY(180deg);
  font-size: 3rem;
  font-weight: 700;
}

.card-symbol-red {
  color: #d32f2f;
}
.card-symbol-black {
  color: #212121;
}
</style>
