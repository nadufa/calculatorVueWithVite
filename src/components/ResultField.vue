<template>
  <div :class="['ResultFieldWrap', { seaTheme: seaTheme }]">
    <div class="ResultField">
      <h1 class="Result">{{ result }}</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: "ResultField",
  props: {
    result: String,
    seaTheme: Boolean,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ResultFieldWrap {
  background-color: rgba(211, 160, 255, 0.5);
  height: 23%;
  width: 98%;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 5px;
}

.seaTheme {
  background-color: #bdcbff;
}

.ResultField {
  background-color: #f8f9ff;
  height: 70%;
  width: 90%;
  display: flex;
  justify-content: end;
  align-items: center;
  padding: 10px;
  overflow: auto;
  border-radius: 10px;
}

.Result {
  font-size: 40px;
}
</style>
