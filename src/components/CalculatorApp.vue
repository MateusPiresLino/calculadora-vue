<template>
  <div class="max-w-sm w-full mx-auto bg-gray-900 rounded-xl shadow-lg p-4">
    <div class="bg-black text-right text-white text-2xl rounded mb-2 px-2 py-4 min-h-[48px]">
      {{ display }}
    </div>
    <!-- botões  -->
    <div class="grid grid-cols-4 gap-2">
      <Button type="action" @click="clearAll">C</Button>
      <Button type="action" @click="inverterSinal">±</Button>
      <Button type="action" @click="porcentagem">%</Button>
      <Button type="operator" @click="chooseOperator('÷')">÷</Button>

      <Button type="action" @click="addDigit('7')">7</Button>
      <Button type="action" @click="addDigit('8')">8</Button>
      <Button type="action" @click="addDigit('9')">9</Button>
      <Button type="operator" @click="chooseOperator('×')">×</Button>

      <Button type="action" @click="addDigit('4')">4</Button>
      <Button type="action" @click="addDigit('5')">5</Button>
      <Button type="action" @click="addDigit('6')">6</Button>
      <Button type="operator" @click="chooseOperator('-')">-</Button>

      <Button type="action" @click="addDigit('1')">1</Button>
      <Button type="action" @click="addDigit('2')">2</Button>
      <Button type="action" @click="addDigit('3')">3</Button>
      <Button type="operator" @click="chooseOperator('+')">+</Button>

      <Button :extra="'col-span-2'" type="action" @click="addDigit('0')">0</Button>
      <Button type="action" @click="addDigit('.')">.</Button>
      <Button type="operator" @click="calculate">=</Button>
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue';
import Button from './Button.vue'

const display = ref('0')
const firstNumber = ref(null)
const operator = ref(null)
const limparDisplayNoProximoNumero = ref(false)

function addDigit(digit) {
  if (limparDisplayNoProximoNumero.value) {
    display.value = digit
    limparDisplayNoProximoNumero.value = false
  } else if (display.value === '0') {
    if (digit === '0') {
      return
    } else if (digit === '.') {
      display.value = '0.'
    } else {
      display.value = digit
    }
  } else {
    display.value += digit
  }
}

function chooseOperator(op) {
  if (firstNumber.value === null) {
    firstNumber.value = display.value
    operator.value = op
    limparDisplayNoProximoNumero.value = true
  } else if (!limparDisplayNoProximoNumero.value) {
    calculate()
    operator.value = op
    limparDisplayNoProximoNumero.value = true
  }
}

function clearAll() {
  display.value = '0'
  firstNumber.value = null
  operator.value = null
  limparDisplayNoProximoNumero.value = false
}

function calculate() {
  const n1 = parseFloat(firstNumber.value)
  const n2 = parseFloat(display.value)
  let result = 0

  switch (operator.value) {
    case '+':
      result = n1 + n2
      break
    case '-':
      result = n1 - n2
      break
    case '×':
      result = n1 * n2
      break
    case '÷':
      result = n1 / n2
      break
    default:
      result = n2
  }

  display.value = result.toString()
  firstNumber.value = result
  operator.value = null
  limparDisplayNoProximoNumero.value = false
}

function inverterSinal() {
  if (display.value !== '0') {
    display.value = (parseFloat(display.value) * -1).toString()
  }
}

function porcentagem() {
  display.value = (parseFloat(display.value) / 100).toString()
  limparDisplayNoProximoNumero.value = true
}

</script>

<style scoped></style>
