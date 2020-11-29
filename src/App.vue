<template>
  <div id="app-container" class>
    <div id="app">
      <h1 id="app-header">Pomodoro Clock</h1>
      <TimerDisplay :label="displayLabel" :time-remaining="timeRemaining" />
      <TimerControls />
      <div id="steps">
        <PomodoroStep label="Session" :length="sessionLength" />
        <PomodoroStep label="Break" :length="breakLength" />
      </div>
    </div>
  </div>
</template>

<script>
import TimerDisplay from "./components/TimerDisplay";
import TimerControls from "./components/TimerControls";
import PomodoroStep from "./components/PomodoroStep";

export default {
  name: "App",
  data() {
    return {
      timeRemaining: "25:00",
      sessionLength: 25,
      breakLength: 5,
      displayLabel: "Session",
      timerRunning: false
    };
  },
  components: {
    TimerDisplay,
    TimerControls,
    PomodoroStep,
  },
};
</script>

<style>
@font-face {
  font-family: digital;
  src: url(./assets/digital-7.ttf);
}

html,
body,
#root {
  height: 100%;
}

body {
  margin: 0;
}

#app-container {
  font-family: "Baloo Bhai", cursive;
  background-color: #87ceeb;
  height: 100%;
  display: flex;
  justify-content: center;
}

#app {
  height: 50%;
  text-align: center;
}

#app-header {
  font-size: 3em;
  margin-bottom: 15px;
}

.control-btn,
.timer-btn {
  background: rgba(135, 206, 235, 0);
  border: none;
  cursor: pointer;
  outline: none;
}
</style>
