<template>
  <div class="modal">
    <article>
      <h2>You Win</h2>
      <p class="time">You Win with {{ attempts }} attempts</p>
      <button @click="shuffleReset">Play again</button>
    </article>
  </div>
</template>

<script setup lang="ts">
const props = defineProps({ reset: Function, attempts: Number });
const emit = defineEmits(["reset"]);
const shuffleReset = () => {
  props.reset();
  emit("reset", false);
};
</script>

<style scoped lang="scss">
@import "./assets/normalize.scss";
.modal {
  font-family: "Poor Story", cursive;
  position: absolute;
  inset: 0;
  background-color: transparent;
  z-index: 999;

  display: grid;
  place-items: center;
}
article {
  background-color: $primary_color;
  padding-inline: 1rem;
  padding-block: 0.5rem;
  border-radius: 0.25rem;

  display: flex;
  place-items: center;
  flex-direction: column;
  gap: 1rem;
  justify-content: space-evenly;

  box-shadow: rgba(50, 50, 93, 0.25) 0rem 0.375rem 0.75rem -0.125rem,
    rgba(0, 0, 0, 0.3) 0rem 0.1875rem 0.4375rem -0.1875rem;
}
button {
  @extend %buttons;
}

h2 {
  @include clamp(2rem, 8vw, 4rem);
  margin-top: 1rem;
  color: $secondary_color;
  font-family: "Shalimar", cursive;
}
.time {
  color: $secondary_color;
  font-size: 1.5rem;
}
</style>
