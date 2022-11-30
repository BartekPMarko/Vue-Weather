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
              {{ weather.name }},{{ weather.sys.country }}
              {{ getTime(weather.timezone) }}
            </div>
            <div class="date">{{ dateBuilder() }}</div>
            <div class="weather-box">
              <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
              <div class="weather">{{ weather.weather[0].description }}</div>
            </div>
          </div>
        </div>
      </div>

    </main>
  </div>

</template>
<script>
export default {

  name: "app",
  data() {
    return {
      api_key: "c6d626d37147d75dc24ec0a54e865843",
      url_base: "http://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      // styles: {
      //   Image: `src/assets/${weather.weather[0].main}.jpg`
      // }
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
<style scoped>
main {
  position: relative;
  height: 100vh;

}

.hehe {
  z-index: 999;
  position: absolute;
  width: 100%;
  height: 100%;
  background-image: url("src/assets/cyberpunk.jpg");
  background-repeat: no-repeat;
  background-size: cover;

}


.blurred-img {
  position: absolute;
  width: 100%;
  height: 100%;
  background-image: inherit;
  background-repeat: no-repeat;
  background-size: cover;
  filter: blur(8px);
  scale: 1.1;
}

.hero-shadow {
  height: 100%;
  position: absolute;
  top: 0;
  border-radius: 30px;
  left: 50%;
  z-index: -1;
  width: 100%;
  padding: 25px;
  background-image: linear-gradient(rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
  transform: translateX(-50%);
}

.hero-img {
  position: relative;
  margin: 0 auto;
  top: 100px;
  height: 600px;
  max-width: 500px;
  z-index: 0;
  border-radius: 30px;
  padding: 25px;
  background-image: inherit;
  background-size: cover;
  background-position: center;
  box-shadow: 0px 0px 30px 10px rgba(0, 0, 0, 0.8);

}

.search-box {
  z-index: 5;
  width: 60%;
  margin: 0 auto;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  font-weight: 600;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px;
  box-shadow: 0px 0px 16px rgba(255, 255, 255, 0.5);
}

.location-box .location {
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: white;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  color: white;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: white;
  font-size: 50px;
  font-weight: 900;
  font-style: italic;
  text-transform: capitalize;
}
</style>
