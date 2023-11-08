<script setup>
  import { reactive } from "vue"
  import  Display from "../components/Display.vue"
  import  Button from "../components/Button.vue"

  const estado = reactive({
    displayValue: '0',
    clearDisplay: false,
    operation: null,
    values: [0, 0],
    current: 0
  })

  const clearMemory = function() {
    Object.assign(estado, {
      displayValue: '0',
      clearDisplay: false,
      operation: null,
      values: [0, 0],
      current: 0
    })
  }

  const setOperation = function(operation) {
    if (estado.current === 0) {
      estado.operation = operation
      estado.current = 1
      estado.clearDisplay = true
    } else {
      const equals = operation === '='
      const currentOperation = estado.operation

      try {
        estado.values[0] = eval(`${estado.values[0]} ${currentOperation} ${estado.values[1]}`)
        if (isNaN(estado.values[0]) || !isFinite(estado.values[0])) {
          estado.clearMemory()
        }
      } catch(e) {
        estado.$emit('error', e)
      }

      estado.values[1] = 0

      estado.displayValue = estado.values[0]
      estado.operation = equals ? null : operation
      estado.current = equals ? 0 : 1
      estado.clearDisplay = !equals
    }
  }

  const addDigit = function(digit) {
    if (digit === '.' && estado.displayValue.includes('.')) {
      return
    }

    const clearDisplay = estado.displayValue === '0' || estado.clearDisplay
    const currentValue = clearDisplay ? '' : estado.displayValue
    const displayValue = currentValue + digit
    
    //Alternativa 1 - para o código abaixo caso queira de fato ter os valores no array no formato string
    estado.displayValue = displayValue
    estado.clearDisplay = false
    estado.values[estado.current] = displayValue


    //Alternativa 2 - para o código acima caso queira de fato ter os valores no array no formato float
    /* if (digit !== '.') {
      const i = estado.current
      const newValue = parseFloat(displayValue)
      estado.values[i] = newValue

    } */
  }

</script>

<template>
  <div class="calculator">
    <Display :value="estado.displayValue" />
    <Button label="AC" triple @onClick="clearMemory"/>
    <Button label="/" operation @onClick="setOperation"/>
    <Button label="7" @onClick="addDigit"/>
    <Button label="8" @onClick="addDigit"/>
    <Button label="9" @onClick="addDigit"/>
    <Button label="*" operation @onClick="setOperation"/>
    <Button label="4" @onClick="addDigit"/>
    <Button label="5" @onClick="addDigit"/>
    <Button label="6" @onClick="addDigit"/>
    <Button label="-" operation @onClick="setOperation"/>
    <Button label="1" @onClick="addDigit"/>
    <Button label="2" @onClick="addDigit"/>
    <Button label="3" @onClick="addDigit"/>
    <Button label="+" operation @onClick="setOperation"/>
    <Button label="0" double @onClick="addDigit"/>
    <Button label="." @onClick="addDigit"/>
    <Button label="=" operation @onClick="setOperation"/>
  </div>
</template>

<style>
  .calculator {
    height: 320px;
    width: 235px;
    border-radius: 5px;
    overflow: hidden;

    display: grid;
    grid-template-columns: repeat(4, 25%);
    grid-template-rows: 1fr 48px 48px 48px 48px 48px;
  }
</style>