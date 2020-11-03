<template>
  <div id="main" :class="isDay ? 'day' : 'night'">
    <div>
      <h1 class="main title text-center">Weather in</h1>
      <form v-on:submit.prevent="getWeather">
        <b-form-input
          type="text"
          class="text-muted my-5 shadow-sm w-25 mx-auto"
          placeholder="Enter city name to view weather"
          v-model="citySearch"
          autocomplete="off"
        />
      </form>
      <p class="text-center my-3" v-if="cityFound">No city found</p>

      <div class="flip-card mx-auto" style="background:red" v-if="visible">
        <div class="flip-card-inner">
          <div class="flip-card-front">
            <p>{{ weather.cityName }}</p>
            <img
              :src="image_url + weather.icon"
              alt="Avatar"
              style="width:300px;height:300px;"
            />
          </div>
          <div class="flip-card-back">
            <p>{{ weather.cityName }}</p>
            <span>{{ weather.temperature }}&deg;F</span>
            <p>{{ weather.lowTemp }}&deg;F</p>
            <p>{{ weather.highTemp }}&deg;F</p>
            <p>{{ weather.feelsLike }}&deg;F</p>
            <p>{{ weather.humidity }}%</p>
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
  mounted() {
    this.image_url = "http://openweathermap.org/img/wn/";
  },
  methods: {
    //fetch and assign weather data
    getWeather: async function() {
      console.log(this.citySearch);
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
}

/* The flip card container - set the width and height to whatever you want. We have added the border property to demonstrate that the flip itself goes out of the box on hover (remove perspective if you don't want the 3D effect */
.flip-card {
  background-color: transparent;
  width: 300px;
  height: 200px;
  border: 1px solid #f1f1f1;
  perspective: 1000px; /* Remove this if you don't want the 3D effect */
}

/* This container is needed to position the front and back side */
.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}

/* Do an horizontal flip when you move the mouse over the flip box container */
.flip-card:hover .flip-card-inner {
  transform: rotateY(180deg);
}

/* Position the front and back side */
.flip-card-front,
.flip-card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  -webkit-backface-visibility: hidden; /* Safari */
  backface-visibility: hidden;
}

/* Style the front side (fallback if image is missing) */
.flip-card-front {
  background-color: #bbb;
  color: black;
}

/* Style the back side */
.flip-card-back {
  background-color: dodgerblue;
  color: white;
  transform: rotateY(180deg);
}
</style>
