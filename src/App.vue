<script>
import CardBlock from "./components/CardBlock";
export default {
  name: "App",
  components: {
    CardBlock,
  },

  setup() {
    const cardList = [];

    for (let i = 0; i < 16; i++) {
      cardList.push(i);
    }

    return {
      cardList,
    };
  },
};
</script>

<template>
  <h1>Memory Game</h1>
  <section class="game-board">
    <CardBlock v-for="(card, index) in cardList" :key="`card-${index}`" :value="card"></CardBlock>
  </section>
</template>

<style>
.card {
  border: 5px solid #ccc;
}
.game-board {
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  justify-content: center;
}
</style>
