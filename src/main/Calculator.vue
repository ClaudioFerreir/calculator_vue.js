<script setup>
  import  Display from "../components/Display.vue"
  import  Button from "../components/Button.vue"
</script>

<template>
  <div class="calculator">
    <Display />
    <Button label="AC" />
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