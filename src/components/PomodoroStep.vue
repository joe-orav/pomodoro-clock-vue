<template>
  <div className="time-length-container">
    <p id="" className="control-label">Set {{ label }} Length:</p>
    <div className="time-length-controls">
      <button id="" className="control-btn">▲</button>
      <div className="length-num-ctr">
        <p id="" className="time-length">{{ length }}</p>
      </div>
      <button id="" className="control-btn">▼</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Pomodoro Step",
  props: {
    label: String,
    length: Number,
  },
};
</script>

<style scoped>
.time-length-container {
  display: flex;
  justify-content: space-between;
  font-size: 1.3em;
}

.control-label,
.time-length,
.time-length-controls {
  display: inline-block;
}

.length-num-ctr {
  width: 50px;
}

.time-length {
  padding: 0px 10px;
  margin: 0;
}

.control-btn {
  font-size: 1.1em;
  border: 1px solid #000;
  border-radius: 5px;
  padding: 0px 10px;
  line-height: 30px;
}

.time-length-controls {
  height: auto;
  display: flex;
  align-items: center;
}
</style>