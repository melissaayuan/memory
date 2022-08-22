<script>
import { ref, watch } from "vue";
import CardBlock from "./components/CardBlock";
export default {
  name: "App",
  components: {
    CardBlock,
  },

  setup() {
    const cardList = ref([]);
    const userSelection = ref([]);
    const status = ref("");

    for (let i = 0; i < 16; i++) {
      cardList.value.push({
        value: i,
        visible: false,
        position: i,
        matched: false,
      });
    }

    const flipCard = (payload) => {
      cardList.value[payload.position].visible = true;

      if (userSelection.value[0]) {
        userSelection.value[1] = payload;
      } else {
        userSelection.value[0] = payload;
      }
    };

    watch(
      userSelection,
      (currentValue) => {
        if (currentValue.length === 2) {
          const cardOne = currentValue[0];
          const cardTwo = currentValue[1];

          if (cardOne.faceValue === cardTwo.faceValue) {
            status.value = "Matched!";
            cardList.value[cardOne.position].matched = true;
            cardList.value[cardTwo.position].matched = true;
          } else {
            status.value = "Mismatch!";

            cardList.value[cardOne.position].visible = false;
            cardList.value[cardTwo.position].visible = false;
          }

          userSelection.value.length = 0;
        }
      },
      { deep: true }
    );

    return {
      cardList,
      flipCard,
      userSelection,
      status,
    };
  },
};
</script>

<template>
  <h1>Memory Game</h1>
  <section class="game-board">
    <CardBlock
      v-for="(card, index) in cardList"
      :key="`card-${index}`"
      :matched="card.matched"
      :value="card.value"
      :visible="card.visible"
      :position="card.position"
      @select-card="flipCard"
    ></CardBlock>
  </section>
  <h2>{{ status }}</h2>
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
