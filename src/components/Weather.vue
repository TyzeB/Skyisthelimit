<template>
  <div class="weather-info">
      <h3>{{ this.temperature }}<br>{{ this.description }}</h3>
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
  },
};
</script>

<style lang="sass" scoped>
@import '../assets/weather-info.scss'
</style>