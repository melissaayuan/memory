<script>
import { computed } from "vue";

export default {
  props: {
    matched: {
      type: Boolean,
      default: false,
    },
    position: {
      type: Number,
      required: true,
    },
    value: {
      type: String,
      required: true,
    },
    visible: {
      type: Boolean,
      default: false,
    },
  },
  setup(props, context) {
    const flippedStyles = computed(() => {
      if (props.visible) {
        return "is-flipped";
      } else {
        return console.log("nothing");
      }
    });

    const selectCard = () => {
      context.emit("select-card", {
        position: props.position,
        faceValue: props.value,
      });
    };
    return {
      flippedStyles,
      selectCard,
    };
  },
};
</script>

<template>
  <div class="card" :class="flippedStyles" @click="selectCard">
    <div class="card-face is-front">
      <img :src="`/images/${value}.png`" alt="value" class="images" />
      <img v-if="matched" src="../../public/images/checkmark.svg" class="icon-checkmark" />
    </div>

    <div class="card-face is-back"></div>
  </div>
</template>

<style>
.card {
  position: relative;
  transition: 0.5s transform ease-in;
  transform-style: preserve-3d;
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  border-radius: 10px;
  backface-visibility: hidden;
}

.card.is-flipped {
  transform: rotateY(180deg);
}

.card-face.is-front {
  background-image: url("../../public/images/purplecheck.png");
  color: black;
  transform: rotateY(180deg);
}

.card-face.is-back {
  background-image: url("../../public/images/backround.png");
  color: white;
}

.icon-checkmark {
  position: absolute;
  width: 25px;
  height: 25px;
  right: 5px;
  bottom: 5px;
}

.images {
  width: 125px;
  height: 125px;
}
</style>
