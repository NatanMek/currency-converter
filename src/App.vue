<template>
  <toolbar-top title="Welcome to the Currency Converter!"></toolbar-top>
  <section>
    <currency-converter></currency-converter>
  </section>
</template>

<script>
import ToolbarTop from './components/UI/ToolbarTop.vue'
import CurrencyConverter from './components/UI/CurrencyConverter.vue'

export default {
  components: {
    ToolbarTop,
    CurrencyConverter
  }
}
</script>

<style scoped>
@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
  }
}
</style>
