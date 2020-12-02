<template>
  <div id="app-container" :class="containerClasses">
    <div id="app">
      <h1 id="app-header">Pomodoro Clock</h1>
      <TimerDisplay :label="displayLabel" :time-remaining="clockTime" />
      <TimerControls
        :toggle-timer="toggleTimer"
        :timer-running="timerRunning"
        :reset-timer="resetTimer"
      />
      <div id="steps">
        <PomodoroStep
          :label="steps.sessionStep.label"
          :length="steps.sessionStep.length"
          :update-length="updateLength"
        />
        <PomodoroStep
          :label="steps.breakStep.label"
          :length="steps.breakStep.length"
          :update-length="updateLength"
        />
      </div>
    </div>
  </div>
</template>

<script>
import TimerDisplay from "./components/TimerDisplay";
import TimerControls from "./components/TimerControls";
import PomodoroStep from "./components/PomodoroStep";
import audioFile from "./assets/soft-bells.mp3";

const DEFAULT_SESSION_TIME = 25;
const DEFAULT_BREAK_TIME = 5;
const DECREMENT_VALUE = 1000;
const alertAudio = new Audio(audioFile);

function convertToMinutes(minutes) {
  return minutes * 60 * 1000;
}

export default {
  name: "App",
  data() {
    return {
      timerIntervalId: null,
      timeRemaining: convertToMinutes(DEFAULT_SESSION_TIME),
      steps: {
        sessionStep: {
          label: "Session",
          length: DEFAULT_SESSION_TIME,
          active: true,
        },
        breakStep: {
          label: "Break",
          length: DEFAULT_BREAK_TIME,
          active: false,
        },
      },
      timerRunning: false,
    };
  },
  computed: {
    clockTime() {
      let newTime = new Date(this.timeRemaining);
      return `${newTime.getMinutes() < 10 ? "0" : ""}${newTime.getMinutes()}:${
        newTime.getSeconds() < 10 ? "0" : ""
      }${newTime.getSeconds()}`;
    },
    containerClasses() {
      return {
        "color-loop-animation":
          this.timerRunning && this.timeRemaining > 10 * 1000,
        "red-color-loop-animation":
          this.timerRunning && this.timeRemaining <= 10 * 1000,
      };
    },
    displayLabel() {
      return this.steps.sessionStep.active
        ? this.steps.sessionStep.label
        : this.steps.breakStep.label;
    },
  },
  methods: {
    toggleTimer() {
      let newTimerState = !this.timerRunning;

      if (newTimerState) {
        this.timerIntervalId = setInterval(() => {
          let newTimeRemaining = this.timeRemaining - DECREMENT_VALUE;

          if (newTimeRemaining === 0) {
            alertAudio.play();
            alertAudio.currentTime = 0;
          }

          if (this.timeRemaining === 0) {
            const { sessionStep, breakStep } = this.steps;

            this.steps = {
              sessionStep: { ...sessionStep, active: !sessionStep.active },
              breakStep: { ...breakStep, active: !breakStep.active },
            };

            this.timeRemaining = convertToMinutes(
              sessionStep.active ? breakStep.length : sessionStep.length
            );
          } else {
            this.timeRemaining = newTimeRemaining;
          }
        }, 1000);
      } else {
        clearInterval(this.timerIntervalId);
      }

      this.timerRunning = newTimerState;
    },
    updateLength(name, newLength) {
      const { sessionStep, breakStep } = this.steps;
      let step = sessionStep.label == name ? sessionStep : breakStep;

      if (!this.timerRunning) {
        step.length = newLength;

        if (step.active) {
          this.timeRemaining = convertToMinutes(newLength);
        }
      }
    },
    resetTimer() {
      clearInterval(this.timerIntervalId);
      this.timeRemaining = convertToMinutes(DEFAULT_SESSION_TIME);
      this.steps = {
        sessionStep: {
          label: "Session",
          length: DEFAULT_SESSION_TIME,
          active: true,
        },
        breakStep: {
          label: "Break",
          length: DEFAULT_BREAK_TIME,
          active: false,
        },
      };
      this.timerRunning = false;
    },
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

@media screen and (max-width: 376px) {
  #app-header {
    font-size: 2em;
  }

  #timer-label {
    font-size: 1.5em;
  }

  #time-left {
    font-size: 4em;
  }

  .time-length-container {
    flex-direction: column;
    align-items: center;
  }
}

.color-loop-animation {
  animation-name: color-loop;
  animation-duration: 50s;
  animation-iteration-count: infinite;
}

@keyframes color-loop {
  0% {
    background-color: #87ceeb;
  }

  10% {
    background-color: #4b0082;
  }

  20% {
    background-color: #0a0aa5;
  }

  30% {
    background-color: #a15304;
  }

  40% {
    background-color: #07a307;
  }

  50% {
    background-color: #aaaa09;
  }

  60% {
    background-color: #019601;
  }

  70% {
    background-color: #a35507;
  }

  80% {
    background-color: #0606ad;
  }

  90% {
    background-color: #4b0082;
  }

  100% {
    background-color: #87ceeb;
  }
}

.red-color-loop-animation {
  animation-name: red-color-loop;
  animation-duration: 1s;
  animation-iteration-count: infinite;
}

@keyframes red-color-loop {
  0% {
    background-color: #ff0000;
  }

  50% {
    background-color: #800000;
  }

  100% {
    background-color: #ff0000;
  }
}
</style>
