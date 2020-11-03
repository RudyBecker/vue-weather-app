<template>
  <div id="main" :class="isDay ? 'day' : 'night'">
    <div>
      <h1 class="main title text-center mt-5">
        Weather in {{ weather.cityName }}
      </h1>
      <form v-on:submit.prevent="getWeather">
        <b-form-input
          type="text"
          class="text-muted font-weight-bold my-5 shadow-sm w-25 mx-auto"
          placeholder="Enter city name to view weather"
          v-model="citySearch"
          autocomplete="off"
        />
      </form>
      <h5 class="text-center my-3" v-if="cityFound">
        No city found, please enter city name.
      </h5>

      <div class="flip-card mx-auto" v-if="visible">
        <div class="flip-card-inner">
          <div class="flip-card-front">
            <img
              :src="imageUrl"
              alt="Avatar"
              style="width:200px;height:200px;"
            />
            <h1 style="font-size:5em">{{ weather.temperature }}&deg;F</h1>
            <!-- <h1 class="mx-auto">{{ weather.cityName }}</h1> -->
          </div>
          <div class="flip-card-back">
            <h5 class="py-1">
              The current temperature in {{ weather.cityName }} is
              {{ weather.temperature }} &deg;F
            </h5>
            <h5 class="py-1">
              You will find {{ weather.description }} currently in
              {{ weather.cityName }}.
            </h5>
            <h5 class="py-1">
              Today's low temperature will be {{ weather.lowTemp }}&deg;F, with
              a high of {{ weather.highTemp }}&deg;F
            </h5>
            <h5 class="py-1">Enjoy your time in {{ weather.cityName }}!</h5>
            <h6 class="text-center font-weight-lighter font-italic">
              Built by Rudy Becker using Vue with data from OpenWeatherMap.org
            </h6>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    cityFound: false,
    visible: false,
    stormy: false,
    cloudy: false,
    clearSky: false,
    clearNight: false,
    snowy: false,
    isDay: true,
    citySearch: "",
    weather: {
      cityName: "",
      country: "",
      temperature: "",
      description: "",
      lowTemp: "",
      highTemp: "",
      feelsLike: "",
      humidity: "",
      icon: "",
    },
    image_url: "",
  }),
  // mounted() {
  //   this.image_url = "http://openweathermap.org/img/wn/";
  // },
  methods: {
    //fetch and assign weather data
    getWeather: async function() {
      console.log(this.citySearch);
      console.log(this.weather.icon);
      const key = "bb047a2b68262787a19ee0c198ae9661";
      const baseURL = `http://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=imperial`;

      try {
        const response = await fetch(baseURL);
        const data = await response.json();
        console.log(data);
        this.citySearch = "";
        this.weather.cityName = data.name;
        this.weather.country = data.sys.country;
        this.weather.temperature = Math.round(data.main.temp);
        this.weather.description = data.weather[0].description;
        this.weather.lowTemp = Math.round(data.main.temp_min);
        this.weather.highTemp = Math.round(data.main.temp_max);
        this.weather.feelsLike = Math.round(data.main.feels_like);
        this.weather.humidity = Math.round(data.main.humidity);

        this.weather.icon = data.weather[0].icon;
        console.log(this.weather.icon);
        // this.weather.iconLink = `http://openweathermap.org/img/wn/${this.weather.icon}@2x.png`;

        const timeOfDay = data.weather[0].icon;

        //check for time of day
        if (timeOfDay.includes("n")) {
          this.isDay = false;
        } else {
          this.isDay = true;
        }

        this.visible = true;
        this.cityFound = false;
      } catch (error) {
        this.cityFound = true;
        this.visible = false;
      }
    },
  },
  // computed: {
  //   iconImage: {
  //     get: function() {
  //       return this.iconLink;
  //     },
  //     set: function() {
  //       this.iconLink = `http://openweathermap.org/img/wn/${this.weather.icon}@2x.png`;
  //     },
  //   },
  // },
  computed: {
    imageUrl() {
      // const image ="http://openweathermap.org/img/wn/" + this.weather.icon + "@2x.png";
      // const weatherIcon = "10d";
      const image = `http://openweathermap.org/img/wn/${this.weather.icon}@2x.png`;

      return image;
    },
  },
};
</script>

<style>
* #main {
  position: absolute;
  height: 100%;
  width: 100%;
  margin: 0 auto;
}
.day {
  background: url(./assets/day.png) no-repeat center center;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}
.night {
  background: url(./assets/night.png) no-repeat center center;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}

.title {
  font-size: 50px;
  font-weight: 500;
  font-family: "Arial Black", Gadget, sans-serif;
}

.flip-card {
  background-color: transparent;
  width: 500px;
  height: 400px;
  perspective: 1000px;
}

.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}

.flip-card:hover .flip-card-inner {
  transform: rotateY(180deg);
}

.flip-card-front,
.flip-card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  -webkit-backface-visibility: hidden; /* Safari */
  backface-visibility: hidden;
}

.flip-card-front {
  background-color: transparent;
  color: black;
}

/* Style the back side */
.flip-card-back {
  background-color: transparent;
  color: white;
  transform: rotateY(180deg);
}
</style>
