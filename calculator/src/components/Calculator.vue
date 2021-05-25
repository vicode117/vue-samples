<template>
  <div class="calculator">
    <div class="display">{{ result }}</div>
    <div @click="clear" class="btn">{{ clearContext }}</div>
    <div @click="sign" class="btn">+/-</div>
    <div @click="percent" class="btn">%</div>
    <div @click="divide" class="btn operator">÷</div>

    <div @click="append('7')" class="btn">7</div>
    <div @click="append('8')" class="btn">8</div>
    <div @click="append('9')" class="btn">9</div>
    <div @click="times" class="btn operator">X</div>

    <div @click="append('4')" class="btn">4</div>
    <div @click="append('5')" class="btn">5</div>
    <div @click="append('6')" class="btn">6</div>
    <div @click="minus" class="btn operator">-</div>

    <div @click="append('1')" class="btn">1</div>
    <div @click="append('2')" class="btn">2</div>
    <div @click="append('3')" class="btn">3</div>
    <div @click="plus" class="btn operator">+</div>

    <div @click="append('0')" class="btn zero">0</div>
    <div @click="dot()" class="btn">.</div>
    <div @click="equal()" class="btn operator">=</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      result: "0",
      clearContext: "AC",
      operator: null,
      oldResult: "",
      operatorValue: '',
      operatorClicked: false,
      resulted: false,
    };
  },
  methods: {
    clear() {
      this.result = '0';
      this.oldResult = '';
      this.operatorValue = '';
      this.operator = null;
      this.clearContext = 'AC';
    },
    sign() {
      this.result =
        this.result.charAt(0) === '-'
          ? this.result.slice(1)
          : `-${this.result}`;
    },
    percent() {
      this.result = `${parseFloat(this.result) / 100}`;
    },
    append(number) {
      this.clearContext = "C";
      if (this.operatorClicked || this.resulted) {
        this.result = "0";
      }

      this.result =
        this.result === "0" && number !== "."
          ? `${number}`
          : `${this.result}${number}`;
      this.operatorValue = this.operatorClicked ? this.result : this.operatorValue;
      this.oldResult = this.resulted ? this.result : this.oldResult;

      this.resulted = false;
      this.operatorClicked = false;
    },
    dot() {
      if (this.result.indexOf(".") === -1) {
        this.append(".");
      }
    },
    setOperator() {
      this.oldResult = this.result;
      this.operatorClicked = true;
    },
    divide() {
      this.operator = (a, b) => a / b;
      this.setOperator();
    },
    times() {
      this.operator = (a, b) => a * b;
      this.setOperator();
    },
    minus() {
      this.operator = (a, b) => a - b;
      this.setOperator();
    },
    plus() {
      this.operator = (a, b) => a + b;
      this.setOperator();
    },
    equal() {
      if(this.operator === null)
      { 
        this.operator = (a,b) => a?a:b;
      }
      this.result = `${this.operator(parseFloat(this.oldResult), parseFloat(this.operatorValue))}`;

      this.resulted = true;
      this.oldResult = this.result;
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calculator {
  margin: 0 auto;
  width: 400px;
  font-size: 3em;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

.display {
  grid-column: 1 / 5;
  background-color: rgba(0, 0, 0, 0.8);
  color: #fff;
}

.zero {
  grid-column: 1 / 3;
  background-color: lightgray;
}

.btn {
  background-color: #eee;
  border: 1px solid black;
}

.operator {
  background-color: orange;
}
</style>
