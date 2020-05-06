<template>
  <div id="app">
    <h1>{{title}}</h1>
    <div class="time">{{formatedTime}}</div>
    <button v-for="(item, index) in items" :key="index" @click.prevent="setTimer(item.seconds, item.format)">{{item.time}}</button>
  </div>
</template>

<script>
// Se debe iniciar la variable antes del conteo, para poder utilizar el clearInterval
let interval

export default {
  name: 'CountDown',
  data () {
    return {
      title: 'Cuenta regresiva',
      formatedTime: '00:00:00 H:min:seg',
      items: [
        {
          time: '3s',
          format: 'seg',
          seconds: 3
        }, {
          time: '1min',
          format: 'min/seg',
          seconds: 60
        }, {
          time: '5m',
          format: 'min/seg',
          seconds: 300
        }, {
          time: '10m',
          format: 'min/seg',
          seconds: 600
        }, {
          time: '30m',
          format: 'min/seg',
          seconds: 1800
        }]
    }
  },

  methods: {
    setTimer (seconds, format) {
      // Aqui se limpia el interval antes de inicar cualquier otra cuenta, esto permite que no se monte un tiempo sobre el otro.
      clearInterval(interval)
      this.formatedTime = ''
      this.countDownLeft(seconds, format)
    },

    countDownLeft (seconds, format) {
      let endDate = Date.now() + ((seconds) * 1000)
      console.log('endDate', endDate)

      interval = setInterval(() => {
        // Calculando tiempo en segundos entre las dos fechas
        let distance = Math.floor(endDate - Date.now() + 1000) / 1000
        // Calculando hora, minutos y segundos
        let hr = ('0' + Math.floor((distance / 3600 % 24))).slice(-2)
        let min = ('0' + Math.floor((distance / 60) % 60)).slice(-2)
        let sec = ('0' + Math.floor(distance % 60)).slice(-2)
        if (distance > 0) {
          // Insertando resultado en variable formatedTime, la cual será renderizada en el template
          this.formatedTime = `${hr}:${min}:${sec} ${format}`
        } else {
          // Parar temporizador cuando la cuenta llegue a 0
          clearInterval(interval)
          this.formatedTime = '¡Tiempo expirado!'
        }
      }, 1)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, .time {
  font-weight: bold;
}

.time {
  margin: 1em;
}

button {
  margin: 1em;
}
</style>
