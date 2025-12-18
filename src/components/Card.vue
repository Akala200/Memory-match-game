<script setup>
defineProps({
  card: {
    type: Object,
    required: true,
  },
  isFlipped: {
    type: Boolean,
    default: false,
  },
  isMatched: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits(['flip']);

const onClick = () => {
  emit('flip');
};
</script>

<template>
  <div
    class="card-container"
    :class="{ flipped: isFlipped || isMatched, matched: isMatched }"
    @click="onClick"
  >
    <div class="card-inner">
      <div class="card-front">
        <!-- Content gets injected here, e.g. an image -->
        <span class="text-4xl">{{ card.content }}</span>
      </div>
      <div class="card-back">
        <!-- Back pattern -->
        <span class="text-2xl">?</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card-container {
  width: 100px;
  height: 100px;
  cursor: pointer;
  perspective: 1000px;
  user-select: none;
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.6s;
  transform-style: preserve-3d;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
}

.card-container.flipped .card-inner {
  transform: rotateY(180deg);
}

.card-front,
.card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 12px;
  font-weight: bold;
}

.card-front {
  background-color: white;
  color: #333;
  transform: rotateY(180deg);
  border: 2px solid #ddd;
}

.card-back {
  background-color: #ff6b6b;
  background-image: 
    radial-gradient(circle at 10% 20%, rgba(255, 255, 255, 0.4) 0%, rgba(255, 255, 255, 0.4) 20%, transparent 20%, transparent 100%),
    radial-gradient(circle at 90% 80%, rgba(255, 255, 255, 0.4) 0%, rgba(255, 255, 255, 0.4) 20%, transparent 20%, transparent 100%);
  background-size: 100% 100%;
  color: white;
  font-size: 2rem;
}

.card-container.matched {
  cursor: default;
}

.card-container.matched .card-front {
  background-color: #a8e6cf; /* Success color */
  border-color: #a8e6cf;
}
</style>
