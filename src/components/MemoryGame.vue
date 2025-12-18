<script setup>
import {ref, onMounted} from 'vue';
import Card from './Card.vue';
import confetti from 'canvas-confetti'; // Adding a fun confetti effect for winning!

// We can add canvas-confetti via npm later, or just mock it for now.
// I'll assume standard imports work or I will install it.
// To be safe, I'll remove the import for now and use a simple alert or just visual change,
// unless I install it. I'll stick to visual change for now to avoid dependency hell,
// or I can add it to package.json. Let's stick to core vue.

const emojis = ['🍎', '🍌', '🍒', '🍇', '🍉', '🍓', '🍍', '🥝'];
const cards = ref([]);
const flippedCards = ref([]);
const isLocked = ref(false);
const moves = ref(0);
const matchedPairs = ref(0);

const initGame = () => {
  const items = [...emojis, ...emojis];
  // Shuffle
  for (let i = items.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [items[i], items[j]] = [items[j], items[i]];
  }
  
  cards.value = items.map((content, index) => ({
    id: index,
    content,
    isFlipped: false,
    isMatched: false,
  }));
  
  flippedCards.value = [];
  moves.value = 0;
  matchedPairs.value = 0;
  isLocked.value = false;
};

const handleFlip = (card) => {
  if (isLocked.value || card.isFlipped || card.isMatched) return;
  
  card.isFlipped = true;
  flippedCards.value.push(card);
  
  if (flippedCards.value.length === 2) {
    isLocked.value = true;
    moves.value++;
    checkMatch();
  }
};

const checkMatch = () => {
  const [card1, card2] = flippedCards.value;
  
  if (card1.content === card2.content) {
    card1.isMatched = true;
    card2.isMatched = true;
    matchedPairs.value++;
    flippedCards.value = [];
    isLocked.value = false;
    
    if (matchedPairs.value === emojis.length) {
      // Game Won!
      setTimeout(() => {
        confetti({
          particleCount: 150,
          spread: 70,
          origin: { y: 0.6 },
          colors: ['#ff6b6b', '#ffd93d', '#4ecdc4', '#45b7d1']
        });
      }, 500);
    }
  } else {
    setTimeout(() => {
      card1.isFlipped = false;
      card2.isFlipped = false;
      flippedCards.value = [];
      isLocked.value = false;
    }, 1000);
  }
};

onMounted(() => {
  initGame();
});
</script>

<template>
  <div class="game-board">
    <div class="header">
      <h1 class="title">Tesla's Memory Match Game</h1>
      <div class="stats">
        <span>Moves: {{ moves }}</span>
        <button class="restart-btn" @click="initGame">Restart 🔄</button>
      </div>
    </div>
    
    <div class="grid">
      <Card
        v-for="card in cards"
        :key="card.id"
        :card="card"
        :isFlipped="card.isFlipped"
        :isMatched="card.isMatched"
        @flip="() => handleFlip(card)"
      />
    </div>
  </div>
</template>

<style scoped>
.game-board {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

.title {
  font-family: 'Comic Sans MS', 'Chalkboard SE', sans-serif;
  color: #ff6b6b;
  font-size: 3rem;
  margin-bottom: 20px;
  text-shadow: 2px 2px 0px #ffd93d;
}

.header {
  margin-bottom: 30px;
}

.stats {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
  font-size: 1.5rem;
  color: #4ecdc4;
}

.restart-btn {
  background-color: #ffd93d;
  border: none;
  padding: 10px 20px;
  border-radius: 20px;
  font-size: 1.2rem;
  cursor: pointer;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  transition: transform 0.2s;
}

.restart-btn:hover {
  transform: scale(1.05);
}

.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 15px;
  justify-items: center;
}

@media (max-width: 600px) {
  .grid {
    grid-template-columns: repeat(4, 1fr); /* Keep 4 for mobile if possible, or 3 */
    /* 4x4 fits ok on mobile if cards are small. 
       If cards are 100px, 400px + gaps is too wide for 320px screens. 
       Need responsive card size or column change. */
  }
  
  .title {
    font-size: 2rem;
  }
  
  :deep(.card-container) {
    width: 70px;
    height: 70px;
  }
  
  :deep(.text-4xl) {
    font-size: 2rem;
  }
}
</style>
