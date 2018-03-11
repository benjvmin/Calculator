<template>
  <div id="app">
    <calculator/>
  </div>
</template>

<script>
import calculator from './components/calculator'

export default {
  name: 'App',
  components: {
    calculator
  }
}
</script>

<style lang="scss">
#app {
  margin: 0;
  padding: 0;
}
</style>
