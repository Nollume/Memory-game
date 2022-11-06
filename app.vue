<template>
  <div class="container">
    <div>
      <h1>Memory Game</h1>
      <div class="info">
        <h2>Attempts {{ attempts }}</h2>
        <button @click="reset()">New Game</button>
      </div>
      <Pexeso
        @win="youWin = $event"
        @reset="reset = $event"
        @attempts="attempts = $event"
      />
      <YouWin
        v-if="youWin"
        :attempts="attempts"
        :reset="reset"
        @reset="youWin = $event"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
const youWin = ref(false);
let reset: Function;
const attempts = ref(0);
</script>

<style lang="scss">
@import "./assets/normalize.scss";

%headline {
  color: $secondary_color;

  text-align: center;
  margin-bottom: 1rem;
}

.container {
  background-color: $primary_color;
  min-height: 100vh;
  display: grid;
  place-items: center;
}
h1 {
  @include clamp(3rem, 8vw, 5rem);
  font-family: "Shalimar", cursive;
  @extend %headline;

  text-transform: uppercase;
}
h2 {
  font-family: "Poor Story", cursive;
  @include clamp(1rem, 5vw, 2rem);
  @extend %headline;
}
.info {
  display: flex;
  align-items: center;
  justify-content: space-around;
  margin-bottom: 1rem;
  & button {
    @extend %buttons;
  }
}
</style>
