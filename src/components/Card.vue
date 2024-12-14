<template>
  <div class="card-container" @click.prevent="cardClickHandler">
    <div
      class="card"
      :class="{
        active: isFliped,
        glowing: isFliped && !isMatched && !isDismatched,
        dismatched: isDismatched,
      }"
    >
      <div class="front"></div>
      <div
        class="back"
        :class="{
          matched: isMatched,
        }"
      >
        <div class="top">
          <span>{{ cardNumberForUI }}</span>
          <cardSymbol :color="cardColor" />
        </div>
        <div class="middle">
          <cardSymbol width="24px" height="24px" :color="cardColor" />
        </div>
        <div class="bottom">
          <span>{{ cardNumberForUI }}</span>
          <cardSymbol :color="cardColor" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import Spade from "../icons/Spade.vue";
import Heart from "../icons/Heart.vue";
import Club from "../icons/Club.vue";
import Diamond from "../icons/Diamond.vue";

enum CardColor {
  red = "#b18d5e",
  // red = "#ec1e23",
  black = "#b18d5e",
}

interface CardEmits {
  (e: "flip", cardNumber: number): void;
}

interface CardProps {
  cardNumber: number;
  isFliped: boolean;
  isMatched: boolean;
  isDismatched: boolean;
  flipArr: number[];
  width?: string;
  height?: string;
}

const props = withDefaults(defineProps<CardProps>(), {
  width: "90px",
  height: "120px",
});

const emit = defineEmits<CardEmits>();

const width = ref(props.width);
const height = ref(props.height);

const cardSymbol = computed(() => {
  switch (Math.floor(props.cardNumber / 13)) {
    case 0:
      return Spade;
    case 1:
      return Heart;
    case 2:
      return Diamond;
    case 3:
      return Club;
  }
});

const cardNumberForUI = computed(() => {
  const num = (props.cardNumber % 13) + 1;

  switch (num) {
    case 1:
      return "A";
    case 11:
      return "J";
    case 12:
      return "Q";
    case 13:
      return "K";
    default:
      return num;
  }
});

const cardColor = computed(() => {
  if ([0, 3].includes(Math.floor(props.cardNumber / 13))) {
    return CardColor.black;
  } else {
    return CardColor.red;
  }
});

function cardClickHandler() {
  if (
    props.flipArr.length >= 2 ||
    props.isMatched ||
    props.flipArr.includes(props.cardNumber)
  )
    return;
  emit("flip", props.cardNumber);
}
</script>

<style scoped lang="scss">
.card-container {
  width: v-bind(width);
  height: v-bind(height);
  perspective: 1000;
  border-radius: 8px;
}

@property --angle {
  syntax: "<angle>";
  initial-value: 0deg;
  inherits: false;
}

.card {
  width: 100%;
  height: 100%;
  transition: all 0.5s ease-in-out;
  transform-style: preserve-3d;
  position: relative;
  border: 2px solid transparent;
  &.active {
    transform: rotateY(180deg);
    & .front {
      z-index: -1;
    }
  }
  &.glowing::after {
    content: "";
    position: absolute;
    height: 100%;
    width: 100%;
    background-image: conic-gradient(
      from var(--angle),
      #c22ed0,
      #5ffae0,
      #c22ed0
    );
    top: 50%;
    left: 50%;
    translate: -50% -50%;
    z-index: -1;
    padding: 2px;
    border-radius: 10px;
    animation: 2s spin linear infinite;
  }
  &.glowing::before {
    filter: blur(1.5rem);
    opacity: 0.5;
  }
}

.front,
.back {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
}

.front {
  background-image: url("/poker.png");
  background-position: center;
  background-size: cover;
  border-radius: 8px;
  width: 100%;
  height: 100%;
  z-index: 2;
  transition: all 0.5s ease-in-out;
}

.back {
  font-size: 1.2rem;
  font-family: "Roboto", serif;
  font-weight: 700;
  color: v-bind(cardColor);
  display: flex;
  flex-direction: column;
  border-radius: 8px;
  background-color: #222;
  transform: rotateY(180deg);
  .top,
  .bottom {
    display: flex;
    flex-direction: column;
    padding: 0 4px;
    span {
      width: 100%;
      height: 20px;
    }
  }
  .bottom {
    align-self: flex-end;
    transform: rotateX(180deg);
  }
  .middle {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
  }
}

.matched {
  background-color: #343434;
}

.dismatched {
  animation-name: shake;
  animation-duration: 0.2s;
  animation-iteration-count: 3;
}

@keyframes spin {
  0% {
    --angle: 0deg;
  }
  100% {
    --angle: 360deg;
  }
}

/* 邊框閃爍效果 */
@keyframes shake {
  0%,
  100% {
    border-color: transparent;
  }
  50% {
    border-color: #b18d5e;
  }
}
</style>
