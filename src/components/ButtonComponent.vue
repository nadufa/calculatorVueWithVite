<template>
  <div class="ButtonWrap">
    <button
      :class="['Button', { Zero: valueForButton === '0', WithoutNumber: !withNumber, seaTheme: seaTheme }]"
      @click="updateResultValue(valueForButton)"
    >
      {{ valueForButton }}
    </button>
  </div>
</template>

<script>
export default {
  name: "ButtonComponent",
  props: {
    valueForButton: String,
    seaTheme: Boolean,
    withNumber: Boolean,
    ["updateResultValue"]: Function,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ButtonWrap {
  display: flex;
  justify-content: center;
  align-items: center;
}

.Button {
  height: 60px;
  width: 70px;
  font-size: 30px;
  background-color: #f8f9ff;
  border-radius: 5px;
  box-shadow: 0 0 3px rgba(0, 0, 0, 0.3);
  border: 2px solid rgb(211, 160, 255);
}

.WithoutNumber {
  background-color: #f7dbff;
}

.seaTheme {
  border: 2px solid #aaa5ff;
}

.seaTheme.WithoutNumber {
  background-color: #cbd7ff;
}

.Zero {
  width: 147px;
}

.Button:hover {
  box-shadow: 0 0 5px rgba(255, 255, 255, 0.8);
}

.Button:active {
  border-width: 3px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(255, 255, 255, 1);
}
</style>
