<template>
  <div class="weather-info">
    <h3>
      {{ this.temperature }}
      <br />
      {{ this.description }}
    </h3>
    <h1>{{ this.city }}</h1>
  </div>
</template>

<script>
export default {
  name: "Weather",
  props: {
    city: String,
    temperature: String,
    description: String,
    image: String
  },
  methods: {
    changeImage: function() {
      const weather = document.querySelector(".weather-info");
      weather.style.backgroundImage = `url("${this.image}")`;
      weather.style.backgroundRepeat = "no-repeat";
      weather.style.backgroundSize = "cover";
    }
  },
  watch: {
    image: function(value) {
      if (value) {
        this.changeImage();
      }
    }
  }
};
</script>

<style lang="sass" scoped>
@import '../assets/weather-info.scss'
</style>