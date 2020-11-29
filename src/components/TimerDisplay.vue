<template>
  <div id="timer-display">
    <h2 id="timer-label">Session</h2>
    <p id="time-left">0:00</p>
  </div>
</template>

<script>
export default {
  name: "TimerDisplay",
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