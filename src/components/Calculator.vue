

<template>
<div class="container">
  <div class="calculator_container">
    <div class="display">
{{ current || '0' }}
    </div>
<div class="keyboard">
<div class="row">
  <button id="clear" @click ="clearAllDisplay()">AC</button>
  <button id="clear" @click ="clearDisplay()" >C</button>
  <button id="memory" v-on:click ="memoryClear()" >MC</button>
  <button id="memory" v-on:click ="memorySave()">MS</button>
  <button id="operator" v-on:click ="handleOperator('×')">×</button>
</div>
<div class="row">
  <button  v-on:click ="appendSymbol('7')">7</button>
  <button  v-on:click ="appendSymbol('8')">8</button>
  <button  v-on:click ="appendSymbol('9')">9</button>
  <button  id="memory" v-on:click ="memoryRecall()">MR</button>
  <button id="operator" v-on:click ="handleOperator('÷')">÷</button>
</div><div class="row">
  <button  v-on:click ="appendSymbol('4')">4</button>
  <button  v-on:click ="appendSymbol('5')">5</button>
  <button  v-on:click ="appendSymbol('6')">6</button>
  <button  id="memory" v-on:click ="memoryMinus()">M-</button>
  <button id="operator" v-on:click ="handleOperator('-')">-</button>
</div><div class="row">
  <button  v-on:click ="appendSymbol('1')">1</button>
  <button  v-on:click ="appendSymbol('2')">2</button>
  <button  v-on:click ="appendSymbol('3')">3</button>
  <button id="memory" v-on:click ="memoryPlus()">M+</button>
  <button id="operator" v-on:click ="handleOperator('+')">+</button>
</div><div class="row">
  <button id="special" v-on:click ="convertDecimal()">.</button>
  <button  v-on:click ="appendSymbol('0')">0</button>
  <button  id="special" v-on:click ="changeSign()">+/-</button>
  <button  id="special" v-on:click ="square()">&#8730</button>

  <button id="operator" v-on:click ="convertPercent()">%</button>
</div>
<div class="last-row">
<button v-on:click ="handleOperator('=')">=</button>
</div>
</div>


  </div>
</div>
</template>
<script>
export default {
  data () {
    return {
      current: '',
      previous: null,
      operator: null,
      clickedOperator: false,
      memory:''
    }
  },
  methods: {
    square(){
      if(this.current > 0){
        this.current = Math.sqrt(this.current)
      }
else{
  this.current='Error'
}
    },
    memoryClear(){
      this.memory=''
    },
    memorySave(){
      this.memory=this.current
    },
    memoryRecall(){
      this.current=this.memory
    },
    memoryMinus(){
      this.current=this.memory - this.current
    },
    memoryPlus(){
      this.current = parseFloat(this.memory) + parseFloat(this.current);
    },
    clearAllDisplay () {
      this.current = ''
      this.previous = null
      this.operator = null
      this.clickedOperator = false
    },
    clearDisplay () {
      this.current= this.current.slice(0,-1);
     
    },
    appendSymbol (number) {
      if (this.clickedOperator) {
        this.clickedOperator = false
        this.current = number
      } else {
        this.current = this.current === '0' ? number : `${this.current}${number}`
      }
    },
    convertDecimal () {
      if (this.clickedOperator) {
        this.current = '0.'
        this.clickedOperator = false
      }
      if (this.current.indexOf('.') === -1) {
        this.current += '0.'
      }
    },
    changeSign () {
      this.current = this.current.charAt(0) === '-' ? this.current.slice(1) : `-${this.current}`
    },
    convertPercent () {
      this.current = `${parseFloat(this.current) / 100}`
    },

    handleOperator (newOperator) {
  const numberValue = parseFloat(this.current)
  if (this.operator && this.clickedOperator) {
    this.operator = newOperator
  }
  if (this.previous == null) {
    this.previous = numberValue
  } else if (this.operator) {
    const result = this.calculate(this.previous, numberValue, this.operator)
    this.current = String(result)
    this.previous = result
  }
  this.clickedOperator = true
  if (newOperator === '=') {
    this.clickedOperator = false
    this.previous = null
  }
  this.operator = newOperator
},
    calculate (first, second, operator) {
      switch (operator) {
        case '+':
          return first + second
        case '-':
          return first - second
        case '×':
          return first * second
        case '÷':
          return first / second
        default:
          return second
      }
    }
  }
}
</script>

<style>

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  font-family: sans-serif;
}

.calculator_container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: currentColor;
  border-radius: 10px;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.2);
  padding: 20px;
}

.display {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  height: 50px;
  width: 100%;
  font-size: 2em;
  padding: 10px;
  box-shadow: inset 0px 0px 10px rgba(0, 0, 0, 0.2);
  margin-bottom: 20px;
  color:#f2f2f2;
}

.keyboard {
  display: grid;
  grid-template-columns: repeat(1, 1fr);
  grid-gap: 10px;
}

.row {
  display: flex;
  justify-content: space-between;
  width: 100%;
  margin-right: 1rem;
}

.last-row {
  display: flex;
  justify-content: space-between;
  width: 100%;
}

.last-row button{
  width: 100%;
  color: white;
  background-color: black;
}

button {
  border: none;
  background-color: #f2f2f2;
  border-radius: 5px;
  height: 50px;
  width: 50px;
  font-size: 1.5em;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
}

#operator,#special{
  color: white;
  background-color: rgb(177, 175, 175);
}
#clear{
  color:white;
  background-color: red;
}

#memory{
  color:white;
  background-color: rgb(239, 174, 10);
}


button:hover {
  background-color: #e6e6e6;
}

button:active {
  transform: scale(0.95);
}

</style>