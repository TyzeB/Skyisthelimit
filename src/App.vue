<template>
  <div id="app">
    <div class="weather">
      <Weather
        :city="finalCity"
        :temperature="temperature"
        :description="description"
        :image="image"
      />
      <div class="search">
        <input id="city-input" type="text" v-model="cityName" />
        <button id="get-weather">
          <i class="fa fa-search"></i>
        </button>
      </div>
    </div>
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
      if (!self.cityName) {
        alert("Please enter city name first");
        return;
      }
      self.fetchData(process.env.VUE_APP_API_URL, self.cityName);
      self.fetchImage(self.cityName);
    });
  },
  data() {
    return {
      cityName: "",
      temperature: "",
      description: "",
      finalCity: "",
      image: ""
    };
  },
  methods: {
    async fetchData(url, city) {
      const self = this;
      await fetch(`${url}q=${city}&appid=${process.env.VUE_APP_API_KEY}&units=metric`)
        .then(response => {
          if (!response.ok) {
            throw Error("Ooops, something went wrong!");
          }
          return response.json();
        })
        .then(data => {
          self.temperature = `${parseInt(data.main.temp).toFixed(0)} °C`;
          self.description = data.weather[0].description.toUpperCase();
          self.finalCity = self.cityName;
        })
        .catch(error => {
          alert(error);
        });
    },
    async fetchImage(city) {
      const self = this;
      city = city.toLowerCase().replace(/\s+/g, "-");
      await fetch(`https://api.teleport.org/api/urban_areas/slug:${city}/images/`)
        .then(response => {
          if (!response.ok) {
            throw Error("Ooops, something went wrong!");
          }
          return response.json();
        })
        .then(data => {
          console.log(data);
          self.image = data.photos[0].image.web;
        })
        .catch(error => {
          alert(error);
        });
    }
  }
};
</script>

<style lang="sass" scoped>
@import 'assets/weather-info.scss'
</style>
