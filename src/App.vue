<template>
  <div :class="['Wrapper', { SeaThemeWrapper: this.state.seaTheme }]">
    <div :class="['Calculator', { SeaThemeCalculator: this.state.seaTheme }]">
      <ResultField :seaTheme="state.seaTheme" :result="state.resultArray.join('')" />
      <MyButtons
          :seaTheme="state.seaTheme"
          :updateResultValue="onButtonClick"
          :valuesForButton="state.valuesForButton"
      />
    </div>
  </div>
</template>

<script>
import MyButtons from "./components/MyButtons.vue";
import ResultField from "./components/ResultField.vue";

export default {
  name: "App",
  components: {
    ResultField,
    MyButtons,
  },
  data() {
    return {
      state: {
        resultArray: [],
        seaTheme: false,
        valuesForButton: [
          [
            {
              withNumber: false,
              valueForButton: "AC",
            },
            {
              withNumber: false,
              valueForButton: "+/-",
            },
            {
              withNumber: false,
              valueForButton: "%",
            },
            {
              withNumber: false,
              valueForButton: "/",
            },
          ],
          [
            {
              withNumber: true,
              valueForButton: "7",
            },
            {
              withNumber: true,
              valueForButton: "8",
            },
            {
              withNumber: true,
              valueForButton: "9",
            },
            {
              withNumber: false,
              valueForButton: "*",
            },
          ],
          [
            {
              withNumber: true,
              valueForButton: "4",
            },
            {
              withNumber: true,
              valueForButton: "5",
            },
            {
              withNumber: true,
              valueForButton: "6",
            },
            {
              withNumber: false,
              valueForButton: "-",
            },
          ],
          [
            {
              withNumber: true,
              valueForButton: "1",
            },
            {
              withNumber: true,
              valueForButton: "2",
            },
            {
              withNumber: true,
              valueForButton: "3",
            },
            {
              withNumber: false,
              valueForButton: "+",
            },
          ],
          [
            {
              withNumber: false,
              valueForButton: "TH",
            },
            {
              withNumber: true,
              valueForButton: "0",
            },
            {
              withNumber: false,
              valueForButton: "=",
            },
          ],
        ],
      },
    };
  },
  methods: {
    onButtonClick(newSign) {
      let expression = this.state.resultArray;

      if (newSign === "AC") {
        clearExpression();
      } else if (newSign === "TH") {
        this.state.seaTheme = !this.state.seaTheme;
      } else if (newSign === "+/-") {
        toggleSign();
      } else if (newSign === "=") {
        calculate();
      } else {
        addToExpression(newSign);
      }

      function addToExpression(value) {
        if (expression.length === 0 && isMathOperator(value) && value !== "-") {
          alert("you can not start expression with this math operator");
        } else {
          const lastElement = expression[expression.length - 1];
          if (
              (lastElement && isMathOperator(lastElement) && isMathOperator(value)) ||
              (expression.length === 1 && expression[0] === "0")
          ) {
            expression[expression.length - 1] = value;
          } else if (expression.length <= 50) {
            expression.push(value);
          }
        }
      }

      function isMathOperator(value) {
        return ["+", "-", "*", "/", ",", "%", ",", "+/-"].includes(value);
      }

      function clearExpression() {
        expression.splice(0, expression.length);
      }

      function toggleSign() {
        const lastElement = expression[expression.length - 1];
        if (!isMathOperator(lastElement)) {
          expression[expression.length - 1] = -parseFloat(lastElement);
        }
      }

      function calculate() {
        let result = calculateExpression();
        clearExpression();

        if (result !== "Error") {
          result = result.toString(); // Преобразование числового результата в строку
          let index = result.indexOf(".");
          if (index !== -1) {
            alert("The result will be rounded");
            result = result.substring(0, index);
          }
          expression.unshift(result);
        } else {
          expression.unshift(result);
        }
      }

      function calculateExpression() {
        let expressionString = expression.join("");
        let result;
        try {
          result = Function(`'use strict'; return (${expressionString})`)();
        } catch (error) {
          result = "Error";
        }

        return result;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: white;
}

.Wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(#f8ddff, #fffef6);
}

.SeaThemeWrapper {
  background: linear-gradient(rgba(15, 14, 31, 0.95), rgba(16, 11, 96, 0.8));
}

.Calculator {
  background-color: #eae3ff;
  height: 500px;
  width: 350px;
  padding: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  border-radius: 10px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.5);
}

.SeaThemeCalculator {
  background-color: #e6fff9;
  box-shadow: 0 0 20px #000000;
}
</style>
