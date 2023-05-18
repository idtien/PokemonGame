<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="handleBeforeStart($event)"
  />
  <interact-screen
    v-else-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen v-else-if="statusMatch === 'finish'" :timer="timer" @onStartAgain="onStartAgain1()" />
  <copy-right />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyRightScreen.vue";
import "./assets/style/global.css";

import { shuffled } from "./utils/array";

export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight,
  },
  data() {
    return {
      settings: {
        totalOfBlock: 0,
        cardsContext: [],
        startAt: null,
      },
      statusMatch: "default",
      timer: 0
    };
  },
  methods: {
    handleBeforeStart(config) {
      this.settings.totalOfBlock = config.totalOfBlocks;

      const firstArrCards = Array.from(
        { length: this.settings.totalOfBlock / 2 },
        (_, i) => i + 1
      );

      const secondsArrCards = [...firstArrCards];

      const mergeCards = [...firstArrCards, ...secondsArrCards];

      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(mergeCards)))
      );

      this.settings.startAt = new Date().getTime();

      this.statusMatch = "match";
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startAt

      this.statusMatch = "finish"
    },
     onStartAgain1: function() {
      console.log("onStartAgain parent");
      this.statusMatch = "default"
     }
  },
};
</script>

<style>
MainScreenstyle > #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
