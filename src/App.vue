<template>
  <div id="app-container" :class="containerClasses">
    <div id="app">
      <h1 id="app-header">Pomodoro Clock</h1>
      <TimerDisplay :label="displayLabel" :time-remaining="timeRemaining" />
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

function decrementTime(time) {
  const [minutes, seconds] = time.split(":");
  let timeInMilliseconds =
    parseInt(minutes) * 60 * 1000 + parseInt(seconds) * 1000 - 1000;
  let newTime = new Date(timeInMilliseconds);

  let newMinutes =
    newTime.getMinutes() < 10
      ? "0" + newTime.getMinutes()
      : newTime.getMinutes();
  let newSeconds =
    newTime.getSeconds() < 10
      ? "0" + newTime.getSeconds()
      : newTime.getSeconds();
  return `${newMinutes}:${newSeconds}`;
}

export default {
  name: "App",
  data() {
    return {
      timerIntervalId: null,
      timeRemaining: "25:00",
      steps: {
        sessionStep: {
          label: "Session",
          length: 25,
          active: true,
        },
        breakStep: {
          label: "Break",
          length: 5,
          active: false,
        },
      },
      timerRunning: false,
    };
  },
  computed: {
    containerClasses() {
      return {
        "color-loop-animation": this.timerRunning,
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
          let newTimeRemaining = decrementTime(this.timeRemaining);

          if (this.timeRemaining === "00:00") {
            const { sessionStep, breakStep } = this.steps;
            if (sessionStep.active) {
              this.steps.sessionStep.active = false;
              this.steps.breakStep.active = true;
              this.timeRemaining = `${breakStep.length < 10 ? "0" : ""}${
                breakStep.length
              }:00`;
            } else {
              this.steps.sessionStep.active = true;
              this.steps.breakStep.active = false;
              this.timeRemaining = `${sessionStep.length < 10 ? "0" : ""}${
                sessionStep.length
              }:00`;
            }
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
          this.timeRemaining = (newLength < 10 ? "0" : "") + newLength + ":00";
        }
      }
    },
    resetTimer() {
      clearInterval(this.timerIntervalId);
      this.timeRemaining = "25:00";
      this.steps = {
        sessionStep: {
          label: "Session",
          length: 25,
          active: true,
        },
        breakStep: {
          label: "Break",
          length: 5,
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
    background-color: #0000ff;
  }

  30% {
    background-color: #ff7f00;
  }

  40% {
    background-color: #00ff00;
  }

  50% {
    background-color: #ffff00;
  }

  60% {
    background-color: #00ff00;
  }

  70% {
    background-color: #ff7f00;
  }

  80% {
    background-color: #0000ff;
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
