<template>
  <div id="app">
    <calculator></calculator>
    <router-view/>
  </div>
</template>

<script>
import Calculator from '@/components/Calculator.vue'

export default {
  name: 'TheApp',
  components: {
    Calculator
  }
}
</script>

<style lang="scss">
body {
  height: 100vh;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
