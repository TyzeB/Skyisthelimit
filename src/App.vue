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
        <button id="get-weather" v-on:click="fetchAll()">
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
    fetchData: function(url, city) {
      const self = this;
      fetch(`${url}q=${city}&appid=${process.env.VUE_APP_API_KEY}&units=metric`)
        .then(async response => {
          const data = await response.json();
          if (!response.ok) {
            const error = (data && data.message) || response.statusText;
            return Promise.reject(error);
          }
          self.temperature = `${parseInt(data.main.temp).toFixed(0)} °C`;
          self.description = data.weather[0].description.toUpperCase();
          self.finalCity = self.cityName;
        })
        .catch(error => {
          alert(error);
        });
    },
    fetchImage: function(city) {
      const self = this;
      city = city.toLowerCase().replace(/\s+/g, "-");
      fetch(`https://api.teleport.org/api/urban_areas/slug:${city}/images/`)
        .then(async response => {
          const data = await response.json();
          if (!response.ok) {
            const error = (data && data.message) || response.statusText;
            return Promise.reject(error);
          }
          self.image = data.photos[0].image.web;
        })
        .catch(error => {
          alert(error);
        });
    },
    fetchAll: function() {
      if (!this.cityName) {
        alert("Please enter city name first");
        return;
      }
      this.fetchImage(this.cityName);
      this.fetchData(process.env.VUE_APP_API_URL, this.cityName);
    }
  }
};
</script>

<style lang="sass" scoped>
@import 'assets/weather-info.scss'
</style>
