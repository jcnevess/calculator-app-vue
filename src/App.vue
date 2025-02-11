<script setup>
import CalculatorScreen from './components/CalculatorScreen.vue'
import CalculatorKey from './components/CalculatorKey.vue'
import { ref } from 'vue'

const screenDisplay = ref('0')
const hasDecimalPoint = ref(false)
const firstTerm = ref('0')
const operator = ref()

const buttonLabels = [
  '7',
  '8',
  '9',
  'del',
  '4',
  '5',
  '6',
  '+',
  '1',
  '2',
  '3',
  '-',
  '.',
  '0',
  '/',
  'x',
  'reset',
  '=',
]

const numericKeys = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
const operatorKeys = ['+', '-', 'x', '/']

function updateScreen(newEntry) {
  if (numericKeys.indexOf(newEntry) > -1) {
    if (screenDisplay.value === '0') {
      screenDisplay.value = newEntry
    } else {
      screenDisplay.value = screenDisplay.value + newEntry
    }
  } else if (operatorKeys.indexOf(newEntry) > -1) {
    hasDecimalPoint.value = false
    operator.value = newEntry
    //save First Term
    firstTerm.value = screenDisplay.value
    screenDisplay.value = '0'
  } else if (newEntry === '=') {
    hasDecimalPoint.value = false
    if (firstTerm.value !== '0' && operator.value != undefined) {
      screenDisplay.value = String(
        performOperation(firstTerm.value, operator.value, screenDisplay.value),
      )
    }
    firstTerm.value = '0'
    operator.value = undefined
  } else if (newEntry === '.') {
    if (hasDecimalPoint.value === false) {
      hasDecimalPoint.value = true
      screenDisplay.value = screenDisplay.value + newEntry
    }
  } else if (newEntry === 'del') {
    if (screenDisplay.value.length > 1) {
      if (screenDisplay.value.slice(-1) === '.') {
        hasDecimalPoint.value = false
      }
      screenDisplay.value = screenDisplay.value.slice(0, -1)
    } else {
      screenDisplay.value = '0'
    }
  } else if (newEntry === 'reset') {
    screenDisplay.value = '0'
    hasDecimalPoint.value = false
    firstTerm.value = '0'
    operator.value = undefined
  }
}

function performOperation(term1, operator, term2) {
  const nterm1 = Number(term1)
  const nterm2 = Number(term2)
  switch (operator) {
    case '+':
      return nterm1 + nterm2
    case '-':
      return nterm1 - nterm2
    case 'x':
      return nterm1 * nterm2
    case '/':
      return nterm1 / nterm2
  }
}
</script>

<template>
  <div class="app">
    <!--header-->
    <div>
      <h1>calc</h1>
    </div>

    <!--screen-->
    <CalculatorScreen :screenDisplay></CalculatorScreen>

    <!--keyboard-->
    <div class="keyboard">
      <CalculatorKey
        v-for="label in buttonLabels"
        :key="label"
        :label
        @key-pressed="updateScreen"
      ></CalculatorKey>
    </div>
  </div>
</template>

<style scoped>
h1 {
  font-size: 1.5rem;
  font-weight: bold;
}

.app {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.keyboard {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0.75rem;
  background-color: var(--color-bg-secondary);
  padding: 1rem;
  border-radius: 5px;
}
</style>
