<template>
  <section>
    <label for="temp">Temperatura (°C):</label>
    <input id="temp" type="number" v-model.number="celsius" />

    <p :style="{ color: colorTexto }">
      Fahrenheit: {{ fahrenheit.toFixed(2) }} °F
    </p>
    <p :style="{ color: colorTexto }">
      Kelvin: {{ kelvin.toFixed(2) }} K
    </p>

    <hr>
    <p>Respuestas multiple choice: 1: c - 2: c - 3: a</p>
  </section>
</template>

<script>
export default {
  name: 'Conversor',
  data() {
    return {
      celsius: 0
    }
  },
  computed: {
    fahrenheit() {
      return (this.celsius * 9 / 5) + 32
    },
    kelvin() {
      return this.celsius + 273.15
    },
    colorTexto() {
      if (this.celsius <= 0) return 'blue'
      if (this.celsius < 15) return 'magenta'
      return 'red'
    }
  }
}
</script>

<style scoped>
label {
  margin-right: 10px;
}
input {
  margin-top: 10px;
}
</style>
