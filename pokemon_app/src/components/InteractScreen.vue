<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((880 - 16 * Math.sqrt(cardContext.length)) /
            Math.sqrt(cardContext.length) -
            16) *
            3) /
            4 +
            32) *
          Math.sqrt(cardContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        :cardContext="cardContext"
        @onFlip="checkRule($event)"
      />
      <set-time-end-game />
    </div>
  </div>
</template>

<script>
import CardFlip from "./CardFlip.vue";
import SetTimeEndGame from "./SetTimeEndGame.vue";

export default {
  props: {
    cardContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
    SetTimeEndGame,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("right");
        //add class disable to component card
        this.$refs[`card-${this.rules[0].index}`][0].onEnabled();
        this.$refs[`card-${this.rules[1].index}`][0].onEnabled();
        //reset rules => []
        this.rules = [];

        const disabledElements = document.querySelectorAll(
          ".screen .card.disabled"
        );

        if (
          disabledElements &&
          disabledElements.length === this.cardContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          //close two card
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();

          //reset rules => []
          this.rules = [];
        }, 800);
      } else return false;
    },
  },
};
</script>
<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}
.screen .screen__inner {
  /* width: 900px; */
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
