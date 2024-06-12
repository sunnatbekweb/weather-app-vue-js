<script>
import axios from "axios";

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },
  computed: {
    cityName() {
      return "<<" + this.city + ">>";
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 3) {
        this.error = "You need to enter more letter!";
        return false;
      } else {
        this.error = "";
        this.info = null;
        axios
          .get(
            `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=f0f6e59dd9133eeb6c2d26a064b9d9ef`
          )
          .then((res) => (this.info = res))
          .catch(
            () => (this.error = `Cannnot get weather in ${this.city}`),
            (this.info = null)
          );
      }
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Weather app</h1>
    <p>Seeing weather in {{ city ? cityName : "your city" }}</p>
    <input type="text" v-model="city" placeholder="Enter city" />
    <button v-if="city != ''" @click="getWeather()">Get weather</button>
    <button v-else disabled>Enter city name</button>
    <p class="error" v-if="error">{{ error }}</p>
    <div v-if="info">
      <p>{{ info.data.weather[0].main }}</p>
      <p>{{ info.data.main.temp }} ^C</p>
      <p>{{ info.data.wind.speed }} m/s</p>
      <p>{{ info.data.sys.country }}</p>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 900px;
  height: 500px;
  background-color: #1f0f24;
  border-radius: 50px;
  padding: 20px;
  color: #ffffff;
  text-align: center;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button {
  background: #e3bc4b;
  color: #ffffff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1);
}

.wrapper button:active {
  transform: scale(0.9);
}

.wrapper button:disabled {
  background: #e3bd4b9d;
  cursor: not-allowed;
}

.wrapper button:disabled:hover {
  transform: scale(1);
}

.wrapper .error {
  color: #d03939;
}
</style>
