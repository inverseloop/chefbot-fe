<template>
  <div class="currentWeather">
<div v-if="forecast">
    <b-carousel id="carousel1"
                style="text-shadow: 1px 1px 2px #333;"
                controls
                indicators
                background="#ababab"
                :interval="4000"
                v-model="slide"
                @sliding-start="onSlideStart"
                @sliding-end="onSlideEnd"
    >
      <b-carousel-slide v-for="(period, key) in forecast" :caption="period.name"
                        :text="period.detailedForecast"
                        :img-src="period.icon"
      ></b-carousel-slide>
    </b-carousel>
  </div>
  <div v-else>
    <button v-on:click="getForecastFor(zip)">Get Forcast for:</button>
    <input v-model="zip" placeholder="Enter Zip Code" />
  </div>
</div>
</template>


<script>
import axios from "axios";

export default {
  data: function() {
    return {
      zip: "",
      latLon: [],
      forecastURL: "",
      forecast: "",
      slide: 0,
      sliding: null
    };
  },
  mounted: function() {
    this.zip = "30075";
  },
  methods: {
    getForecastFor: async function(zip) {
	    let response = await axios.get("http://10.0.0.220:1337/api/v1/zip/" + zip);
	    this.latLon = response.data.latLon;
	    response = await axios.get("https://api.weather.gov/points/" + this.latLon.toString())
            this.forecastURL = response.data.properties.forecast;
	    response = await axios.get(this.forecastURL)
            this.forecast = response.data.properties.periods;
    },
    onSlideStart (slide) {
      this.sliding = true
    },
    onSlideEnd (slide) {
      this.sliding = false
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
