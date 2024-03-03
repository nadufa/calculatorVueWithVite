<template>
  <div :class="['Wrapper', { SeaThemeWrapper: this.state.seaTheme }]">
    <div :class="['Calculator', { SeaThemeCalculator: this.state.seaTheme }]">
      <ResultField :needResultScroll :seaTheme="state.seaTheme" :result="state.resultArray.join('')"/>
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
      if (newSign === "AC") {
        this.state.resultArray = clearExpression(this.state.resultArray);
      } else if (newSign === "TH") {
        this.state.seaTheme = !this.state.seaTheme;
      } else if (newSign === "+/-") {
        this.state.resultArray = toggleSign(this.state.resultArray);
      } else if (newSign === "=") {
        this.state.resultArray = calculateExpression(this.state.resultArray);
      } else {
        this.state.resultArray = addToExpression(this.state.resultArray, newSign);
      }

      function addToExpression(expression, value) {
        let newExpression = [...expression]
        if (newExpression.length === 0 && isMathOperator(value) && value !== "-") {
          alert("you can not start expression with this math operator");
        } else if([...newExpression].join("") === "Infinity") {
          newExpression = addToExpression([], value)
        } else {
          const lastElement = newExpression[newExpression.length - 1];
          if (
              (lastElement && isMathOperator(lastElement) && isMathOperator(value)) ||
              (newExpression.length === 1 && (newExpression[0] === "0"))
          ) {
            newExpression[newExpression.length - 1] = value;
          } else {
            newExpression.push(value);
          }
          }
        return newExpression
      }

      function isMathOperator(value) {
        return ["+", "-", "*", "/", "%"].includes(value);
      }

      function clearExpression(expression) {
        let newExpression = [...expression]
        newExpression.splice(0, newExpression.length);
        return newExpression
      }

      function toggleSign(expression) {
        let newExpression = [...expression]
        const lastElement = newExpression[newExpression.length - 1];
        if (!isMathOperator(lastElement)) {
          newExpression[newExpression.length - 1] = -parseFloat(lastElement);
        }
        return newExpression
      }

      function calculateExpression(expression) {
        let newExpression = [...expression]
        let arrOfOperators = ["%", "*", "/", "+", "-"]

        for(let operatorPosition = 0; operatorPosition < arrOfOperators.length; operatorPosition++) {
          let currentOperator = arrOfOperators[operatorPosition]
          for(let i = 0; i < newExpression.length; i++) {
            if(newExpression[i] === currentOperator) {
              newExpression = universalCase(currentOperator, newExpression)
            }
          }
        }

        console.log('newExpression: ', newExpression)
        return newExpression
      }

      function universalCase(sign, expression) {
        let newExpression = [...expression]
        for (let i = 0; i < newExpression.length; i++) {
          if (newExpression[i] === sign) {
            let numBefore = []
            let numAfter = []
            let startCase = null
            let endCase = null
            for (let j = i - 1; !isMathOperator(newExpression[j]) && j >= 0; j--) {
              numBefore.unshift(newExpression[j])
              startCase = j
            }
            for (let j = i + 1; !isMathOperator(newExpression[j]) && j < newExpression.length; j++) {
              numAfter.push(newExpression[j])
              endCase = j
            }
            let firstStr = numBefore.join('')
            let secondStr = numAfter.join('')
            let resultCase = switchCalculate(sign, firstStr, secondStr).toString().split('')
            newExpression.splice(startCase, endCase - startCase + 1, ...resultCase)
          }
        }
        return newExpression
      }

      function switchCalculate(sign, firstStr, secondStr) {
        switch (sign) {
          case "%": {
            return (+firstStr * +secondStr / 100)
          }
          case "*": {
            return (+firstStr * +secondStr)
          }
          case "/": {
            return (+firstStr / +secondStr)
          }
          case "+": {
            return (+firstStr + +secondStr)
          }
          case "-": {
            return (+firstStr - +secondStr)
          }
        }
      }
    }
  },
}

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
