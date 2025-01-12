<template>
  <section>
    <div>
      <div class="title">
        <img src="/logo.png" alt="logo" />
        <span>Memorizing Game</span>
      </div>
      <div class="score-part">
        <div class="score">
          Score: <span class="text-color">{{ score }}</span>
        </div>
        <div>
          You've tried <span class="text-color">{{ tryCount }}</span> times
        </div>
      </div>
    </div>
    <div class="card-grid">
      <Card
        v-for="card in shuffledCardData"
        :key="card.number"
        :cardNumber="card.number"
        :isFliped="card.isFliped"
        :isMatched="card.isMatched"
        :isDismatched="card.isDismatched"
        :flipArr="flipArr"
        @flip="cardFlipHandler"
      />
    </div>
    <div v-if="allFliped" class="game-over-container">
      <div class="game-over-panel">
        <h1>GAME OVER!</h1>
        <button @click="resetHandler">Restart</button>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, reactive, onMounted, watch, computed } from "vue";
import Card from "./Card.vue";

interface CardItemData {
  number: number;
  isFliped: boolean;
  isMatched: boolean;
  isDismatched: boolean;
}

const tryCount = ref(0);
const score = ref(0);
const flipArr = ref<number[]>([]);

// 所有卡牌資料
const cardData = reactive<CardItemData[]>(
  Array.from({ length: 52 }, (_, i) => ({
    number: i,
    isFliped: false,
    isMatched: false,
    isDismatched: false,
  }))
);

// 洗過牌的所有卡牌
const shuffledCardData = ref<CardItemData[]>([]);

// 所有牌都已經翻開 (遊戲結束)
const allFliped = computed(() => {
  return shuffledCardData.value.every((item) => item.isFliped);
});

/** 翻牌方法 */
function cardFlipHandler(cardNumber: number) {
  flipArr.value = [...flipArr.value, cardNumber];

  const currCard = cardData.find((item) => item.number === cardNumber);

  if (!currCard) return;

  currCard.isFliped = true;
}

/** 隨機洗牌方法 */
function shuffleArray<T>(arr: T[]) {
  for (let i = 0; i < arr.length - 1; i++) {
    const element = arr.shift();

    if (element) {
      const randomIndex = Math.floor(Math.random() * (arr.length - i)) + i;

      arr.splice(randomIndex, 0, element);
    }
  }

  return arr;
}

/** 重新開始 */
function resetHandler() {
  cardData.forEach((item) => {
    item.isFliped = false;
    item.isMatched = false;
    item.isDismatched = false;
  });

  shuffledCardData.value = shuffleArray(cardData);
  tryCount.value = 0;
  score.value = 0;
}

/** 比對 */
watch(flipArr, async () => {
  if (flipArr.value.length !== 2) return;

  tryCount.value++;

  await new Promise((resolve) => setTimeout(resolve, 500));

  const first = flipArr.value[0] % 13;
  const second = flipArr.value[1] % 13;

  const firstCard = cardData.find((card) => card.number === flipArr.value[0]);
  const secondCard = cardData.find((card) => card.number === flipArr.value[1]);

  if (!firstCard || !secondCard) return;

  if (first === second) {
    firstCard.isMatched = true;
    secondCard.isMatched = true;
    score.value += 20;

    flipArr.value = [];
  } else {
    firstCard.isDismatched = true;
    secondCard.isDismatched = true;

    setTimeout(() => {
      firstCard.isDismatched = false;
      secondCard.isDismatched = false;
      firstCard.isFliped = false;
      secondCard.isFliped = false;

      flipArr.value = [];
    }, 600);
  }
});

onMounted(() => {
  shuffledCardData.value = shuffleArray(cardData);
});
</script>

<style scoped>
section {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: #222;
  display: flex;
  flex-direction: column;
}

.title {
  height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 12px;
  font-family: "DM Serif Display", serif;

  img {
    height: 36px;
  }

  span {
    font-size: 2.5rem;
    color: #e0e0e0;
  }
}

.card-grid {
  margin: 0.5rem auto 0;
  width: 1314px;
  flex: 1;
  padding: 1rem 0;
  display: grid;
  justify-content: center;
  align-items: center;
  grid-template-columns: repeat(13, 90px);
  grid-template-rows: repeat(4, 120px);
  gap: 12px;
}

.score-part {
  margin: 3rem auto 0;
  width: 1314px;
  display: flex;
  justify-content: end;
  align-items: center;
  gap: 2rem;
  font-size: 1.4rem;
  color: #e0e0e0;
  font-family: "DM Serif Display", serif;
}

.text-color {
  color: #b18d5e;
}

.game-over-container {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 9999;
  width: 100vw;
  height: 100vh;
  background-color: transparent;
  pointer-events: all;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ffffff60;
  backdrop-filter: blur(5px);
}

.game-over-panel {
  width: 450px;
  height: 450px;
  background-color: #00000090;
  backdrop-filter: blur(5px);
  border-radius: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
}

.game-over-panel h1 {
  font-size: 3rem;
  color: #e0e0e0;
}

.game-over-panel button {
  position: absolute;
  width: calc(100% - 2rem);
  height: 3rem;
  border-radius: 0.5rem;
  border: 0px;
  bottom: 1rem;
  left: 0;
  margin: 0 1rem;
  cursor: pointer;
  color: #fff;
  font-size: 1.25rem;
  background: #b18d5e;
}
</style>
