<script>
import _ from "lodash";
import { computed, ref, watch } from "vue";
import { launchConfetti } from "./utilities/confetti";
import CardBlock from "./components/CardBlock";
export default {
  name: "App",
  components: {
    CardBlock,
  },

  setup() {
    const cardList = ref([]);
    const userSelection = ref([]);
    const newPlayer = ref(true);

    const startGame = () => {
      newPlayer.value = false;

      restartGame();
    };

    const status = computed(() => {
      if (remainingPairs.value === 0) {
        return "Player wins!";
      } else {
        return `Remaining Pairs: ${remainingPairs.value}`;
      }
    });

    const remainingPairs = computed(() => {
      const remainingCards = cardList.value.filter((card) => card.matched === false).length;

      return remainingCards / 2;
    });

    const restartGame = () => {
      cardList.value = _.shuffle(cardList.value);

      cardList.value = cardList.value.map((card, index) => {
        return {
          ...card,
          matched: false,
          position: index,
          visible: false,
        };
      });
    };

    const cardItems = ["unicorn", "sun", "icecreamcone", "raincloud", "rainbow", "crown", "popsicle", "cloud2"];

    cardItems.forEach((item) => {
      cardList.value.push({
        value: item,
        variant: 1,
        visible: false,
        position: null,
        matched: false,
      });
      cardList.value.push({
        value: item,
        variant: 2,
        visible: true,
        position: null,
        matched: false,
      });
    });

    cardList.value = cardList.value.map((card, index) => {
      return {
        ...card,
        position: index,
      };
    });

    const flipCard = (payload) => {
      cardList.value[payload.position].visible = true;

      if (userSelection.value[0]) {
        if (
          (userSelection.value[0].position === payload.position && userSelection.value[0].faceValue) ===
          payload.faceValue
        ) {
          return;
        } else {
          userSelection.value[1] = payload;
        }
      } else {
        userSelection.value[0] = payload;
      }
    };

    watch(remainingPairs, (currentValue) => {
      if (currentValue === 0) {
        launchConfetti();
      }
    });

    watch(
      userSelection,
      (currentValue) => {
        if (currentValue.length === 2) {
          const cardOne = currentValue[0];
          const cardTwo = currentValue[1];

          if (cardOne.faceValue === cardTwo.faceValue) {
            cardList.value[cardOne.position].matched = true;
            cardList.value[cardTwo.position].matched = true;
          } else {
            setTimeout(() => {
              cardList.value[cardOne.position].visible = false;
              cardList.value[cardTwo.position].visible = false;
            }, 2000);
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
      restartGame,
      startGame,
      newPlayer,
    };
  },
};
</script>

<template>
  <h1>Memory Game</h1>
  <transition-group tag="section" class="game-board" name="shuffle-card">
    <CardBlock
      v-for="card in cardList"
      :key="`${card.value}-${card.variant}`"
      :matched="card.matched"
      :value="card.value"
      :visible="card.visible"
      :position="card.position"
      @select-card="flipCard"
    ></CardBlock>
  </transition-group>
  <h2>{{ status }}</h2>
  <button v-if="newPlayer" v-on:click="startGame" class="button">Start Game</button>
  <button v-else v-on:click="restartGame" class="button">Restart Game</button>
</template>

<style>
html,
body {
  margin: 0;
  padding: 0;
}

h1 {
  margin-top: 0;
  color: #778899;
}

h2 {
  color: #778899;
}

#app {
  text-align: center;
  background-color: #add8e6;
  height: 100vh;
  padding-top: 60px;
}

.game-board {
  display: grid;
  grid-template-columns: repeat(4, 120px);
  grid-template-rows: repeat(4, 120px);
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  justify-content: center;
}

.button {
  color: black;
  font-weight: bold;
  background-color: white;
}

.shuffle-card-move {
  transition: transform 0.8s ease-in;
}
</style>
