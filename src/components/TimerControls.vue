<template>
  <div id="timer-controls">
    <button id="start_stop" className="timer-btn" @click="toggleTimer">
      <img :src="toggleBtn" alt="" />
    </button>
    <button id="reset" className="timer-btn">
      <img :src="reloadImg" alt="reset" title="Reset" @click="resetTimer" />
    </button>
  </div>
</template>

<script>
import reloadImg from "../assets/img/reload.png";
import playImg from "../assets/img/play.png";
import pauseImg from "../assets/img/pause.png";
export default {
  name: "TimerControls",
  props: {
    toggleTimer: Function,
    timerRunning: Boolean,
    resetTimer: Function
  },
  data() {
    return {
      reloadImg,
      playImg,
    };
  },
  computed: {
    toggleBtn() {
      return this.timerRunning ? pauseImg : playImg;
    },
  },
};
</script>

<style scoped>
#timer-controls {
  margin-top: 10px;
}

.timer-btn {
  margin: 5px;
  height: 40px;
  width: 40px;
}

.timer-btn img {
  display: block;
  margin: auto;
  height: auto;
  max-width: 100%;
}
</style>