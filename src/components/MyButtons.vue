<template>
  <div class="myButtons">
    <div class="line" v-for="(line, index) in valuesForButton" :key="index">
      <ButtonComponent
        v-for="(buttonObj, index) in line"
        :key="index"
        :seaTheme="seaTheme"
        :updateResultValue="updateResultValue"
        :valueForButton="buttonObj.valueForButton"
        :withNumber="buttonObj.withNumber"
      />
    </div>
  </div>
</template>

<script>
import ButtonComponent from "./ButtonComponent.vue";

export default {
  name: "MyButtons",
  props: {
    valuesForButton: Array,
    seaTheme: Boolean,
    ["updateResultValue"]: Function,
  },
  components: {
    ButtonComponent,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

.myButtons {
  padding: 5px;
  border-radius: 5px;
  height: 73%;
  width: 98%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}

.line {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
</style>
