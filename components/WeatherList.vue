<template>
  <div>
    <h2>Weather {{ selectedCity? `of ${selectedCity}` : '' }}</h2>
    <p v-if="isOffline"> You are in offline mode π΄ Only saved data can be shown</p>
    <div v-if="weather">
      <h2 v-if="isOffline">THIS IS SAVED WEATHER DATA</h2>
      <h3>Temperature π‘οΈ </h3>
      <p>{{ weather.temperature }}</p>
      <h3>Wind π¨</h3>
      <p>{{ weather.wind }}</p>
      <h3>Description π</h3>
      <p>{{ weather.description }}</p>
      <h3>Forecast π</h3>
      <ul>
        <li v-for="day, index in weather.forecast" :key="index">
          <h4>{{ `weather of +${day.day} day after today: temperature is ${day.temperature} and wind is ${day.wind}` }}
          </h4>
        </li>
      </ul>
    </div>
    <div v-if="isFail && isOffline">
      <p>You are in Offline and Weather of {{ selectedCity }} is not saved so can not show anything π </p>
    </div>
    <h2>
      <ul>
        <li v-for="(city, index) in cities" :key="index" @click="fetchWeather(city)">
          {{ city }}
        </li>
      </ul>
    </h2>
    <h2>
      <NuxtLink to="/">Home</NuxtLink>
    </h2>
  </div>
</template>

<script>

export default {
  name: 'WeatherList',
  data() {
    return {
      weather: null,
      selectedCity: null,
      isOffline: false,
      isFail: false,
      cities: [
        'Seoul',
        'Tokyo',
        'Paris',
        'London',
        'Beijing',
      ]
    }
  },
  mounted() {
    this.isOffline = !navigator.onLine
  },
  methods: {
    fetchWeather(city) {
      // eslint-disable-next-line no-console
      console.log(city)
      this.selectedCity = city
      this.$axios.get(`https://goweather.herokuapp.com/weather/${city}`).then(response => {
        if (response.status === 200) {
          this.isFail = false
          this.weather = response.data
          // localForage is a wrapper around IndexedDB
          // λ°μ΄ν°λ₯Ό λΆλ¬μ€λλ° μ±κ³΅νλ©΄, λ°μ΄ν°λ₯Ό μ μ₯νλ€.
          this.$localForage.setItem(city, response.data)
        } else {
          this.$localForage.getItem(city).then(data => {
            this.isFail = false
            this.weather = data
          })
        }
      }).catch(e => {
        // eslint-disable-next-line no-console
        console.error('error: ', e)
        this.$localForage.getItem(city).then(data => {
          this.isFail = false
          this.weather = data
        }).catch(e => {
          // eslint-disable-next-line no-console
          console.error(e)
          this.weather = null
          this.isFail = true
        })
      })
    }
  },
}
</script>


<style>
.nuxt-logo {
  height: 180px;
}
</style>
