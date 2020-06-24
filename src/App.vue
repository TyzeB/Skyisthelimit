<template>
  <div id="app" class="weather">
    <input id="city-input" type="text" v-model="cityName" />
    <button id="get-weather">GET</button>
    <Weather :city="cityName" :temperature="temperature" :description="description" />
  </div>
</template>

<script>
import Weather from "./components/Weather.vue";

export default {
  name: "App",
  components: {
   Weather
  },
  mounted() {
    const submit = document.querySelector("#get-weather");
    const self = this;
    submit.addEventListener("click", () => {
      self.fetchData(process.env.VUE_APP_API_URL, self.cityName);
    });
  },
  data() {
    return {
      cityName: "",
      temperature: "",
      description: ""
    };
  },
  methods: {
    async fetchData(url, city) {
      const response = await fetch(`${url}q=${city}&appid=${process.env.VUE_APP_API_KEY}&units=metric`);
      const data = await response.json();
      this.temperature = `${parseInt(data.main.temp).toFixed(0)} °C`;
      this.description = data.weather[0].description.toUpperCase();
    }
  }
};
</script>

<style lang="sass" scoped>
@import 'assets/weather-info.scss'
</style>
