<template>
  <div class="container">
    <h1>Conversor a dólares</h1>
    <Conversor />
  </div>
</template>

<script>
import Conversor from './components/Conversor.vue'

export default {
  name: 'App',
  components: { Conversor }
}
</script>

<style scoped>
.container {
  font-family: Arial, sans-serif;
  margin: 30px;
}
</style>
