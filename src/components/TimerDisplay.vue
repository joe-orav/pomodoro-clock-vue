<template>
  <div id="timer-display">
    <h2 id="timer-label">{{ label }}</h2>
    <p id="time-left">{{ timeRemaining }}</p>
  </div>
</template>

<script>
export default {
  name: "TimerDisplay",
  props: {
    timeRemaining: String,
    label: String
  },
};
</script>

<style scoped>
#timer-display {
  border: 1px solid #000;
  border-radius: 25px;
}

#timer-label,
#time-left {
  margin: 0;
}

#timer-label {
  font-size: 2em;
  margin-top: 20px;
}

#time-left {
  font-size: 75px;
  font-family: digital, monospace;
  margin-bottom: 20px;
  line-height: 74px;
}
</style>