<template>
  <section>
    <div>
      <div class="title">
        <img src="/logo.png" alt="logo" />
        <span> Memorizing Game </span>
      </div>
      <div class="score">Score: {{ score }}</div>
      <div>You've tried {{ tryCount }} times</div>
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
  </section>
</template>

<script setup lang="ts">
import { ref, reactive, onMounted, watch } from "vue";
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

const cardData = reactive<CardItemData[]>(
  Array.from({ length: 52 }, (_, i) => ({
    number: i,
    isFliped: false,
    isMatched: false,
    isDismatched: false,
  }))
);

const shuffledCardData = ref<CardItemData[]>([]);

function cardFlipHandler(cardNumber: number) {
  flipArr.value = [...flipArr.value, cardNumber];

  const currCard = cardData.find((item) => item.number === cardNumber);

  if (!currCard) return;

  currCard.isFliped = true;
}

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

<style>
section {
  width: 100%;
  height: 100%;
  background-color: #222;
  display: flex;
  flex-direction: column;
}

.title {
  display: flex;
  justify-content: center;
  align-items: center;
  img {
    height: 36px;
  }
  span {
    font-size: 2.5rem;
  }
}

.card-grid {
  width: 100%;
  flex: 1;
  padding: 1rem 0;
  display: grid;
  justify-content: center;
  align-items: center;
  grid-template-columns: repeat(13, 110px);
  grid-template-rows: repeat(4, 150px);
  gap: 12px;
}
</style>
