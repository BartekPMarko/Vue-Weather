<template>
  <div id="app">
    <main
      :style="
      typeof weather.main != 'undefined' ? { backgroundImage: `url(src/assets/${weather.weather[0].main}.jpg)` } : { backgroundImage: `url(src/assets/clear.jpg)` }">
      <div class="hehe" v-if="query == 'Night City'"></div>

      <div class=" blurred-img">
      </div>
      <div class="hero-img">
        <div class="hero-shadow"></div>

        <div class="search-box">
          <input type="text" class="search-bar" placeholder="Wyszukaj..." v-model="query" @keypress="fetchWeather" />
        </div>
        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
          <div class="location-box">
            <div class="location">
              {{ weather.name }}
              {{ getTime(weather.timezone) }}
            </div>
            <div class="date">{{ dateBuilder() }}</div>
            <div class="weather-box">
              <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
              <div class="weather">{{ weather.weather[0].description }}</div>
              <div class="feels-like">Temperatura odczuwalna:
                <div>{{ Math.round(weather.main.feels_like) }}°c</div>
              </div>
            </div>
          </div>
        </div>
      </div>

    </main>
  </div>

</template>
<script>
import './assets/styles/style.css'
export default {

  name: "app",
  data() {
    return {
      api_key: "c6d626d37147d75dc24ec0a54e865843",
      url_base: "http://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&lang=pl&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "Styczeń",
        "Luty",
        "Marzec",
        "Kwiecień",
        "Maj",
        "Czerwiec",
        "Lipiec",
        "Sierpień",
        "Wrzesień",
        "Październik",
        "Listopad",
        "Grudzień",
      ];
      let days = [
        "Niedziela",
        "Poniedziałek",
        "Wtorek",
        "Środa",
        "Czwartek",
        "Piątek",
        "Sobota",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
    getTime(timezone) {
      const localTime = new Date().getTime();
      const localOffset = new Date().getTimezoneOffset() * 60000;
      const currentUtcTime = localOffset + localTime;
      const cityOffset = currentUtcTime + 1000 * timezone;
      const cityTime = new Date(cityOffset).toTimeString().split(" ");
      return cityTime[0];
    },
  },

};


</script>
