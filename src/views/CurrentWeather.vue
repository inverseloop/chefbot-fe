<template>
  <div class="currentWeather">
   <CurrentWeather msg="" />
  </div>
</template>

<script>
// @ is an alias to /src
import CurrentWeather from "@/components/CurrentWeather.vue";

export default {
  name: "currentWeather",
  components: {
    CurrentWeather
  }
};
</script>
