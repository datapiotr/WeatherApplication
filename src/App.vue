<template>
  <main>
    <section class="search-section">
      <input
        type="text"
        class="search-input"
        placeholder="Type the city"
        v-model="city"
        v-on:keypress="getWeather"
      />
      <button class="search-btn" v-on:click="getWeather">Check</button>
    </section>
    <div class="information" v-if="typeof weather.main != 'undefined'">
      <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
      <div class="temperature">{{ Math.round(weather.main.temp) }}°c</div>
      <div class="weather">{{ weather.weather[0].description }}</div>
      <div class="other">
        <div class="perceived">
          Perceived temperature: {{ Math.round(weather.main.feels_like) }}°c
        </div>
        <div class="pressure">
          Pressure: {{ Math.round(weather.main.pressure) }} hPa
        </div>
        <div class="humidity">
          Humidity: {{ Math.round(weather.main.humidity) }}%
        </div>
        <div class="wind">Wind: {{ Math.round(weather.wind.speed) }} m/s</div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "027ee07fafd9a678d925c3a9220c1289",
      url_base: "https://api.openweathermap.org/data/2.5/",
      city: "",
      weather: {},
    };
  },
  methods: {
    getWeather(e) {
      if (e.key == "Enter" || e.type == "click") {
        fetch(
          `${this.url_base}weather?q=${this.city}&units=metric&APPID=${this.api_key}`
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
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, Helvetica, sans-serif;
}

main {
  height: 100vh;
  padding: 30px;
  background-color: #485461;
  background-image: linear-gradient(315deg, #485461 0%, #28313b 74%);
}

.search-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 50px;
}

.search-input {
  width: 100%;
  padding: 20px;
  border: none;
  outline: none;
  box-shadow: 0 0 5px 0.5px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  font-size: 25px;
  transition: 0.3s;
}

.search-input:focus {
  background-color: rgba(255, 255, 255, 0.75);
}

.search-btn {
  min-width: 20%;
  margin-top: 30px;
  padding: 20px;
  border: none;
  border-radius: 50px;
  box-shadow: 0 0 5px 0.5px rgba(0, 0, 0, 0.25);
  font-size: 20px;
  color: white;
  background-color: black;
  cursor: pointer;
  transition: ease 0.6s;
}

.search-btn:hover {
  color: black;
  background-color: white;
}

.information {
  color: white;
  text-align: center;
}

.information div {
  margin-bottom: 30px;
}

.information .other div {
  margin-bottom: 15px;
}

.location {
  font-size: 50px;
  font-weight: 700;
  text-shadow: 2px 3px 5px rgba(0, 0, 0, 0.5);
}

.temperature {
  font-size: 100px;
}

.weather {
  font-size: 25px;
  font-weight: 400;
}
</style>
