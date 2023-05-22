<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardContext="setting.cardContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
  <coppy-right-screen />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CoppyRightScreen from "./components/CoppyRightScreen.vue";

import { shuffled } from "./utils/array";

export default {
  name: "App",
  data() {
    return {
      setting: {
        totalOfBlock: 0,
        cardContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CoppyRightScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("running handle before start....", config);
      this.setting.totalOfBlock = config.totalOfBlocks;
      const firstCard = Array.from(
        { length: this.setting.totalOfBlock / 2 },
        (_, i) => i + 1
      );
      const secondCard = [...firstCard];
      const Card = [...firstCard, ...secondCard];
      this.setting.cardContext = shuffled(shuffled(shuffled(shuffled(Card))));
      console.log(this.setting.cardContext);
      this.setting.startedAt = new Date().getTime();
      //data ready
      this.statusMatch = "match";
    },
    onGetResult() {
      //get time
      this.timer = new Date().getTime() - this.setting.startedAt;
      //switch
      this.statusMatch = "result";
    },
  },
};
</script>
