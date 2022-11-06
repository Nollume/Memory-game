<template>
  <ul>
    <li
      v-for="card in data"
      :key="card.id"
      @click="opencard(card)"
      class="flip-card"
      :style="
        card.cardOpen || card.sameCard
          ? 'transform :rotateY(180deg)'
          : 'transform :rotateY(0deg)'
      "
    >
      <div
        class="flip-card-inner"
        :style="
          card.cardOpen || card.sameCard
            ? 'transform :rotateY(180deg)'
            : 'transform :rotateY(0deg)'
        "
      >
        <div class="flip-card-front">
          <p class="back">pexeso</p>
        </div>
        <div
          class="flip-card-back"
          :class="{
            animationFindCard: card.sameCard,
          }"
        >
          <img :src="card.image" :alt="card.title" />
        </div>
      </div>
    </li>
  </ul>
</template>

<script setup lang="ts">
import { pexesoData, pexeso } from "~/data";
let data = reactive(pexesoData);
const youWin = ref(false);
const attempts = ref(0);

const emit = defineEmits(["win", "reset", "attempts"]);

const shuffleCards = () => {
  return data.sort(() => Math.random() - 0.5);
};

const reset = () => {
  data = shuffleCards();
  data.map((card) => (card.sameCard = false));
  data.map((card) => (card.cardOpen = false));
  attempts.value = 0;
  emit("attempts", attempts.value);
};

const opencards = () => {
  return data.filter((item) => item.cardOpen);
};
const findCards = () => {
  return data.filter((item) => item.sameCard);
};
const ifWin = () => {
  return data.every((item) => item.sameCard);
};

const opencard = (card: pexeso) => {
  if (
    opencards().some((item) => item.id === card.id) ||
    findCards().some((item) => item.id === card.id)
  )
    return;

  if (opencards().length < 2) {
    card.cardOpen = true;
  }

  if (opencards().length === 2) {
    const openCards = data.filter((item) => item.cardOpen);
    attempts.value++;
    emit("attempts", attempts.value);
    setTimeout(() => {
      openCards.map((openCard) => {
        openCard.cardOpen = false;

        if (openCards[1].title === openCards[0].title) {
          openCard.sameCard = true;
        }
        if (ifWin()) {
          youWin.value = true;
          emit("win", youWin.value);
        }
      });
    }, 1000);
  }
};

onMounted(() => {
  reset();
  emit("reset", reset);
});
</script>

<style scoped lang="scss">
@import "./assets/normalize.scss";
h3 {
  font-size: 2rem;
  text-align: center;
}
ul {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0.5rem;
  margin-inline: 0.5rem;
  & li {
    width: clamp(4rem, 10vw, 7rem);
    aspect-ratio: 1/1;

    cursor: pointer;
    transition: all 0.3s ease-in;
  }
}

img {
  width: 80%;
  height: auto;
}
.back {
  font-family: "Shalimar", cursive;
  rotate: 45deg;
  font-size: 1.5rem;
  user-select: none;
}

.flip-card {
  background-color: transparent;
  perspective: 1000px;
}

.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}

.flip-card:hover .flip-card-inner {
  &:is(:hover, :active) .flip-card-front {
    opacity: 0.8;
  }
  &:is(:hover, :active) .flip-card-back {
    opacity: 0.8;
  }
}

.flip-card-front,
.flip-card-back {
  display: grid;
  place-items: center;
  position: absolute;
  width: 100%;
  height: 100%;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
}

.flip-card-front {
  background-color: $accent_color;
  box-shadow: rgba(50, 50, 93, 0.25) 0rem 0.375rem 0.75rem -0.125rem,
    rgba(0, 0, 0, 0.3) 0rem 0.1875rem 0.4375rem -0.1875rem;
  border-radius: 0.25rem;
}

.flip-card-back {
  border-radius: 0.25rem;
  box-shadow: rgba(50, 50, 93, 0.25) 0rem 0.375rem 0.75rem -0.125rem,
    rgba(0, 0, 0, 0.3) 0rem 0.1875rem 0.4375rem -0.1875rem;
  background-color: $accent_color;
  transform: rotateY(180deg);
}

.animationFindCard {
  background-color: rgba(128, 223, 128, 0.889);
  animation: visibility 0.5s forwards ease-in-out;
}

@keyframes visibility {
  0% {
    opacity: 0;
  }

  100% {
    opacity: 1;
  }
}

@media screen and (min-width: 760px) {
  ul {
    grid-template-columns: repeat(5, 1fr);
  }
}
</style>
