<template>
  <section>
    <p>
      Ingrese monto $ 
      <input
        type="number"
        v-model.number="pesos"
        min="0"
      />
    </p>

    <p>
      Valor del dólar en $ 
      <input
        type="number"
        v-model.number="cotizacionDolar"
        :disabled="usarAutomatico"
        min="0"
        step="0.01"
      />
      - Actualización
      <input
        type="checkbox"
        v-model="usarAutomatico"
      />
      <span v-if="ultimaActualizacion">
        {{ fechaHoraFormateada }}
      </span>
    </p>

    <p>
      Valor convertido USD {{ dolares.toFixed(2) }}
    </p>

    <hr />
    <p>
      Respuestas multiple choice:
      1: C y F | 2: B | 3: C | 4: A | 5: B
    </p>
  </section>
</template>


<script>
import axios from 'axios'

export default {
  name: 'Conversor',
  data() {
    return {
      pesos: 0,
      cotizacionDolar: 0,         
      usarAutomatico: false,      
      ultimaActualizacion: null,   
      intervalId: null,         
      error: null
    }
  },
  computed: {
    dolares() {
      if (!this.cotizacionDolar || !this.pesos) return 0
      return this.pesos / this.cotizacionDolar
    },
    fechaHoraFormateada() {
      if (!this.ultimaActualizacion) return ''
      const d = this.ultimaActualizacion
      const dia = String(d.getDate()).padStart(2, '0')
      const mes = String(d.getMonth() + 1).padStart(2, '0')
      const anio = d.getFullYear()
      const horas = String(d.getHours()).padStart(2, '0')
      const minutos = String(d.getMinutes()).padStart(2, '0')
      const segundos = String(d.getSeconds()).padStart(2, '0')
      return `${dia}/${mes}/${anio} ${horas}:${minutos}:${segundos}`
    }
  },
  methods: {
    async obtenerCotizacion() {
      try {
        this.error = null
        const response = await axios.get('https://api.bluelytics.com.ar/v2/latest')
        this.cotizacionDolar = response.data.oficial.value_sell
        this.ultimaActualizacion = new Date()
      } catch (e) {
        this.error = 'No se pudo obtener la cotización'
        console.error(e)
      }
    },
    iniciarActualizacionAutomatica() {
      this.obtenerCotizacion()
      this.intervalId = setInterval(() => {
        this.obtenerCotizacion()
      }, 2000)
    },
    detenerActualizacionAutomatica() {
      if (this.intervalId) {
        clearInterval(this.intervalId)
        this.intervalId = null
      }
    }
  },
  watch: {
    usarAutomatico(nuevoValor) {
      if (nuevoValor) {
        this.detenerActualizacionAutomatica()
        this.iniciarActualizacionAutomatica()
      } else {
        this.detenerActualizacionAutomatica()
      }
    }
  },
  beforeUnmount() {
    this.detenerActualizacionAutomatica()
  }
}
</script>

<style scoped>
label {
  margin-right: 10px;
}
input {
  margin-top: 5px;
}
section {
  font-family: Arial, sans-serif;
}
</style>
