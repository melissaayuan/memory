<script>
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
    const selectCard = () => {
      context.emit("select-card", {
        position: props.position,
        faceValue: props.value,
      });
    };
    return {
      selectCard,
    };
  },
};
</script>

<template>
  <div class="card" @click="selectCard">
    <div v-if="visible" class="card-face is-front">
      <img :src="`/images/${value}.png`" alt="value" class="images" />
      <img v-if="matched" src="../../public/images/checkmark.svg" class="icon-checkmark" />
    </div>

    <div v-else class="card-face is-back"></div>
  </div>
</template>

<style>
.card {
  position: relative;
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  border-radius: 10px;
}

.card-face.is-front {
  background-image: url("../../public/images/purplecheck.png");
  color: black;
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
