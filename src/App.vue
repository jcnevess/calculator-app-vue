<script setup>
import CalculatorScreen from './components/CalculatorScreen.vue'
import CalculatorKey from './components/CalculatorKey.vue'
import { onErrorCaptured, ref } from 'vue'

const themeNumber = ref('1')
const screenDisplay = ref('0')
const resetScreen = ref(false)
const hasDecimalPoint = ref(false)
const firstTerm = ref()
const operator = ref()
const errorState = ref(false)

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

onErrorCaptured(() => {
  errorState.value = true
  screenDisplay.value = 'error'
  hasDecimalPoint.value = false
  firstTerm.value = undefined
  operator.value = undefined
  resetScreen.value = true
  return false
})

function updateScreen(newEntry) {
  errorState.value = false

  if (numericKeys.indexOf(newEntry) > -1) {
    if (screenDisplay.value === '0' || resetScreen.value) {
      screenDisplay.value = newEntry
      resetScreen.value = false
    } else {
      screenDisplay.value = screenDisplay.value + newEntry
    }

    //
  } else if (operatorKeys.indexOf(newEntry) > -1) {
    if (firstTerm.value !== undefined && operator.value !== undefined) {
      screenDisplay.value = String(
        performOperation(firstTerm.value, operator.value, screenDisplay.value),
      )
    }
    hasDecimalPoint.value = false
    operator.value = newEntry
    firstTerm.value = screenDisplay.value
    resetScreen.value = true

    //
  } else if (newEntry === '=') {
    hasDecimalPoint.value = false
    if (firstTerm.value !== undefined && operator.value !== undefined) {
      screenDisplay.value = String(
        performOperation(firstTerm.value, operator.value, screenDisplay.value),
      )
    }
    firstTerm.value = undefined
    operator.value = undefined
    resetScreen.value = true

    //
  } else if (newEntry === '.') {
    if (hasDecimalPoint.value === false) {
      screenDisplay.value = resetScreen.value ? '0' + newEntry : screenDisplay.value + newEntry
      resetScreen.value = false
      hasDecimalPoint.value = true
    }

    //
  } else if (newEntry === 'del') {
    if (screenDisplay.value === 'error' || screenDisplay.value.length <= 1) {
      screenDisplay.value = '0'
    } else {
      if (screenDisplay.value.slice(-1) === '.') {
        hasDecimalPoint.value = false
      }
      screenDisplay.value = screenDisplay.value.slice(0, -1)
    }

    //
  } else if (newEntry === 'reset') {
    screenDisplay.value = '0'
    hasDecimalPoint.value = false
    firstTerm.value = undefined
    operator.value = undefined
    resetScreen.value = false
  }
}

function performOperation(term1, operator, term2) {
  const nterm1 = Number(term1)
  const nterm2 = Number(term2)

  let result
  switch (operator) {
    case '+':
      result = nterm1 + nterm2
      break
    case '-':
      result = nterm1 - nterm2
      break
    case 'x':
      result = nterm1 * nterm2
      break
    case '/':
      result = nterm1 / nterm2
  }

  if (!isFinite(result)) {
    throw new Error('math')
  }

  return result
}
</script>

<template>
  <div class="app" :class="`theme-${themeNumber}`">
    <!--header-->
    <div class="header">
      <h1>calc</h1>
      <div class="toggler">
        <div class="toggler-header">theme</div>
        <div class="toggler-actions">
          <form action="">
            <div class="toggler-actions-labels">
              <label for="theme-1">1</label>
              <label for="theme-2">2</label>
              <label for="theme-3">3</label>
            </div>
            <div class="toggler-actions-controls">
              <div class="control-container">
                <input type="radio" id="theme-1" value="1" v-model="themeNumber" />
                <span class="checkmark"></span>
              </div>
              <div class="control-container">
                <input type="radio" id="theme-2" value="2" v-model="themeNumber" />
                <span class="checkmark"></span>
              </div>
              <div class="control-container">
                <input type="radio" id="theme-3" value="3" v-model="themeNumber" />
                <span class="checkmark"></span>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>

    <!--screen-->
    <CalculatorScreen :screenDisplay></CalculatorScreen>

    <!--keyboard-->
    <div class="keyboard">
      <CalculatorKey
        v-for="label in buttonLabels"
        :key="label"
        :class="{ highlighted: label === operator }"
        :disabled="errorState && (operatorKeys.indexOf(label) > -1 || label === '=')"
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
  width: 320px;
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.toggler {
  display: flex;
  align-items: center;
  font-size: 0.6rem;
  gap: 1rem;
}

.toggler-header {
  text-transform: uppercase;
  font-weight: bold;
  letter-spacing: 0.25ch;
}

.toggler-actions-labels {
  display: flex;
  justify-content: space-around;
}

.toggler-actions-labels label {
  cursor: pointer;
}

.toggler-actions-controls {
  display: flex;
  flex-direction: row;
  gap: 3px;
  background-color: var(--color-bg-secondary);
  padding: 3px;
  border-radius: 10px;
}

.control-container {
  position: relative;
  display: block;
  width: 10px;
  height: 10px;
  -webkit-user-select: none;
  user-select: none;
}

.control-container input {
  position: absolute;
  z-index: 1;
  opacity: 0;
  cursor: pointer;
  width: 10px;
  height: 10px;
}

.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 10px;
  width: 10px;
  background-color: transparent;
  border-radius: 50%;
}

.control-container input:checked ~ .checkmark {
  background-color: var(--color-bg-equal-key);
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
