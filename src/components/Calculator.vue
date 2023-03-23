

<template>
<div class="container">
  <div class="calculator_container">
    <div class="display">
{{ current || '0' }}
    </div>
<div class="keyboard">
<div class="row">
  <button @click ="clearAllDisplay()">AC</button>
  <button @click ="clearDisplay()" >C</button>
  <button v-on:click ="memoryClear()" >MC</button>
  <button  v-on:click ="memorySave()">MS</button>
  <button  v-on:click ="handleOperator('×')">×</button>
</div>
<div class="row">
  <button  v-on:click ="appendSymbol('7')">7</button>
  <button  v-on:click ="appendSymbol('8')">8</button>
  <button  v-on:click ="appendSymbol('9')">9</button>
  <button  v-on:click ="memoryRecall()">MR</button>
  <button  v-on:click ="handleOperator('÷')">÷</button>
</div><div class="row">
  <button  v-on:click ="appendSymbol('4')">4</button>
  <button  v-on:click ="appendSymbol('5')">5</button>
  <button  v-on:click ="appendSymbol('6')">6</button>
  <button  v-on:click ="memoryMinus()">M-</button>
  <button  v-on:click ="handleOperator('-')">-</button>
</div><div class="row">
  <button  v-on:click ="appendSymbol('1')">1</button>
  <button  v-on:click ="appendSymbol('2')">2</button>
  <button  v-on:click ="appendSymbol('3')">3</button>
  <button  v-on:click ="memoryPlus()">M+</button>
  <button  v-on:click ="handleOperator('+')">+</button>
</div><div class="row">
  <button  v-on:click ="convertDecimal()">.</button>
  <button  v-on:click ="appendSymbol('0')">0</button>
  <button  v-on:click ="changeSign()">+/-</button>
  <button  v-on:click ="square()">&#8730</button>

  <button  v-on:click ="convertPercent()">%</button>
</div>
<button  v-on:click ="handleOperator('=')">=</button>

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