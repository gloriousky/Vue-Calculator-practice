<template>
  <div class="p-3" style="max-width:400px;margin:36px auto; background: #234;">
    <div class="w-full rounded m-1 p-3 text-end lead font-weight-bold text-white bg-vue-dark" >
      {{ showCalculatorValue || 0}}
    </div>
    <!-- Calculator Result -->
    <div class="w-full rounded m-1 p-3 text-end lead font-weight-bold text-white bg-vue-dark" >
      {{ calculatorValue || 0}}
    </div>

    <!-- Calculator buttons -->
    <div class="row gx-0">
      <div class="col-3" v-for="value in calculatorElement" :key="value">
        <div class="text-white lead text-center m-1 py-3 bg-vue-dark rounded hover-btn"
        :class="{'bg-vue-green': ['C', '*','/','-','+','%','='].includes(value)}"
         @click="action(value)"
         >
        {{ value }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  props: {
    msg: String
  },
  data () {
    return {
      showCalculatorValue: '',
      calculatorValue: '',
      previousCalculatorValue: '',
      calculatorElement: ['C', '*', '/', '-', 7, 8, 9, '+', 4, 5, 6, '%', 1, 2, 3, '=', 0, '.'],
      operator: null
    }
  },
  methods: {
    action (n) {
      // Append value
      if (!isNaN(n) || n === '.' || ['/', '*', '+', '-'].includes(n)) {
        this.showCalculatorValue += n + ''
      }
      // Append value
      if (!isNaN(n) || n === '.') {
        this.calculatorValue += n + ''
      }
      // Clear values
      if (n === 'C') {
        this.calculatorValue = ''
        this.showCalculatorValue = ''
      }
      // Percentage
      if (n === '%') {
        this.calculatorValue = this.calculatorValue / 100 + ''
      }
      if (['/', '*', '+', '-'].includes(n)) {
        this.operator = n
        this.previousCalculatorValue = this.calculatorValue
        this.calculatorValue = ''
      }
      if (n === '=') {
        // eslint-disable-next-line no-eval
        this.calculatorValue = eval(
          this.previousCalculatorValue + this.operator + this.calculatorValue
        )
        // eslint-disable-next-line no-eval
        this.showCalculatorValue = this.calculatorValue
        this.previousCalculatorValue = ''
        this.operator = null
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .bg-vue-dark {
    background-color: #31475e
  }
  .hover-btn:hover {
    cursor: pointer;
    background: #3D5875
  }
  .bg-vue-green {
    background: #3fb984
  }
</style>
