
<script>
import Keyboard from '@/components/Keyboard.vue'

export default {
  name: 'TheCalculator',

  components: {
    Keyboard
  },

  data () {
    return {
      result: '0',
      leftOperand: 0,
      rightOperand: 0,
      operator: ''
    }
  },

  computed: {
    hasresultStartMinus () {
      return this.result.startsWith('-')
    },
    visibleResult () {
      const resToFixed = String(Number(this.result).toFixed(3)).replace('.', ',')
      return resToFixed.endsWith(',000') ? resToFixed.split(',')[0] : resToFixed
    }
  },

  methods: {
    reset () {
      if (this.rightOperand) {
        this.rightOperand = 0
        this.operator = ''
      } else {
        this.leftOperand = 0
        this.operator = ''
      }
      this.result = 0
    },

    chackNegative () {
      if (!this.leftOperand) {
      // нет левого
        this.result = this.hasresultStartMinus ? this.result.replace('-', '') : '-' + this.result
      } else if (this.leftOperand && !this.operator) {
        // есть левый и нет оператора
        this.leftOperand = this.hasresultStartMinus ? this.leftOperand.replace('-', '') : '-' + this.leftOperand
        this.result = this.leftOperand
      } else if (this.leftOperand && this.operator) {
        // есть левый и оператор
        this.result = '-0'
        this.rightOperand = this.hasresultStartMinus ? this.result.replace('-', '') : '-' + this.result
        // this.result = this.rightOperand
      } else if (this.rightOperand) {
        // есть правый
        this.result = '-0'
        this.rightOperand = this.rightOperand - (this.rightOperansd * 2)
        console.log('right')
        this.result = this.leftOperand + this.rightOperand
        console.log(' tab right')
      }
    },

    checkPercent () {
      this.result = this.leftOperand / 100
      this.leftOperand = this.result
      this.operator = ''
    },
    checkOperator (value) {
      if (this.leftOperand && !this.rightOperand) {
        // левый и нет правого
        this.operator = value
      } else if (this.rightOperand) {
        // правый
        this.calculation()
        this.operator = value // XM
      }
    },

    checkNUmber (value) {
      if (!Number(this.result) && !this.leftOperand) {
        // нет левого
        this.leftOperand = this.hasresultStartMinus ? '-' + value : value
        this.result = this.leftOperand
      } else if (!this.operator && !this.rightOperand) {
        // нет оператора и нет правого
        this.leftOperand += value
        this.result = this.leftOperand
      }
      if (this.operator && this.leftOperand) {
        // есть опер и есть левый
        if (!this.rightOperand) {
          // есть опер и  нет правого
          this.rightOperand = value
          this.result = this.rightOperand
        } else if (!Number(this.result) && this.rightOperand) {
          // привели к булеан 0 или -0 и есть правый
          this.rightOperand = this.hasresultStartMinus ? '-' + value : this.rightOperand.replace('-', '')
          this.result = this.rightOperand
        } else if (this.rightOperand) {
          this.rightOperand += value
          this.result = this.rightOperand
        }
      }
    },
    checkPoint () {
      console.log()
    },
    change (value) {
      if (this.checkType(value) === 'result' && this.rightOperand) {
        this.calculation()
        this.operator = ''

        return
      }

      const actions = {
        reset: this.reset(),
        negative: this.chackNegative(),
        operator: this.checkOperator(value),
        percent: this.checkPercent(),
        number: this.checkNUmber(value),
        point: this.checkPoint()
      }
      actions[value]()
    },

    checkType (symbol) {
      const dictSymbol = [
        { value: '=', type: 'result' },
        { value: 'AC', type: 'reset' },
        { value: ['+/-'], type: 'negative' },
        { value: ['%'], type: 'percent' },
        { value: ['+', '-', '*', '/'], type: 'operator' },
        { value: ['7', '8', '9', '4', '5', '6', '1', '2', '3', '0'], type: 'number' },
        { value: ',', type: 'point' }
      ]

      for (const { value, type } of dictSymbol) {
        if (value.includes(symbol)) {
          return type
        }
      }
    },

    calculation () {
      const leftNum = parseInt(this.leftOperand)
      const rightNum = parseInt(this.rightOperand)

      switch (this.operator) {
        case '/':
          console.log('/')
          this.result = String(leftNum / rightNum)
          this.rightOperand = ''
          this.leftOperand = this.result
          break
        case '*':
          console.log('*')
          this.result = String(leftNum * rightNum)
          this.rightOperand = ''
          this.leftOperand = this.result
          break
        case '-':
          console.log('-')
          this.result = String(leftNum - rightNum)
          this.rightOperand = ''
          this.leftOperand = this.result
          break
        case '+':
          console.log('+')
          this.result = String(leftNum + rightNum)
          this.rightOperand = ''
          this.leftOperand = this.result
          break
      }
    }
  }
}
</script>

<template>
  <div class="container">
    <div class="body">
      <div class="result">
        <span>{{ visibleResult }}</span>
      </div>
      <keyboard
      @tap="change"
      ></keyboard>
    </div>
    <pre>visibalResult: {{ visibleResult }} {{ typeof visibleResult }}</pre>
    <pre>result: {{ result }} {{ typeof result }}</pre>
    <pre>Left: {{ leftOperand }} {{ typeof leftOperand }}</pre>
    <pre>Right: {{ rightOperand }} {{ typeof rightOperand }}</pre>
    <pre>Operator: {{ operator }} {{ typeof operator }}</pre>
  </div>
</template>

<style scoped lang="scss">
  * {
    margin: 0;
    padding: 0;
  }
  .container {
    display: flex;
    justify-content: center;
    flex-direction:column ;
  }
  .body {
    height: 600px;
    width: 414px;
    font-family: Arial, Helvetica, sans-serif;
    background: #000000;
    color: white;
    border-radius:30px;

  }
  .result {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    span {
      justify-self: flex-end;
      font-size: 100px;
      margin-right: 30px;
    }
  }
  pre {
    font-size: 30px;
  }
</style>
