<template>
  <div id="main" :class="isDay ? 'day' : 'night'">
    <div class="container my-5">
      <h1 class="title text-center">Weather in</h1>
      <p>{{ weather.cityName }}</p>
      <p class="">{{ weather.country }}</p>
      <span>{{ weather.temperature }}&deg;C</span>
      <p class="my-4">{{ weather.description }}</p>
      <p>
        <!-- <img src="./assets/down.svg" alt="" /> -->
        {{ weather.lowTemp }}&deg;C
      </p>
      <p>
        <!-- <img src="./assets/up.svg" alt="" /> -->
        {{ weather.highTemp }}&deg;C
      </p>
      <div class="col text-center">
        <p>{{ weather.feelsLike }}&deg;C</p>
        <span>Feels like</span>
      </div>
      <div class="col text-center">
        <p>{{ weather.humidity }}%</p>
        <span>humidity</span>
      </div>
      <!-- <form class="search-location" v-on:submit.prevent="getWeather">
        <input
          type="text"
          class="form-control text-muted form-rounded p-4 shadow-sm"
          placeholder="What City?"
          v-model="citySearch"
          autocomplete="off"
        />
      </form> -->
      <!-- <p class="text-center my-3" v-if="cityFound">No city found</p> -->
      <div
        class="card rounded my-3 shadow-lg back-card overflow-hidden"
        v-if="visible"
      >
        <!-- weather animation container -->
        <!-- <div>
          <div icon="sunny" v-if="clearSky">
            <span class="sun"></span>
          </div>

          <div icon="snowy" v-if="snowy">
            <ul>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
            </ul>
          </div>

          <div icon="stormy" v-if="stormy">
            <span class="cloud"></span>
            <ul>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
            </ul>
          </div>
          <div icon="cloudy" v-if="cloudy">
            <span class="cloud"></span>
            <span class="cloud"></span>
          </div>
          <div icon="nightmoon" v-if="clearNight">
            <span class="moon"></span>
            <span class="meteor"></span>
          </div>
        </div> -->

        <!-- Top of card starts here -->
        <div class="card-top text-center" style="margin-bottom: 15rem">
          <div class="city-name my-3">
            <p>{{ weather.cityName }}</p>
            <span>...</span>
            <p class="">{{ weather.country }}</p>
          </div>
        </div>
        <!-- top of card ends here -->

        <!--card middle body, card body used cos I want to just update the innerHTML -->
        <div class="card-body">
          <!-- card middle starts here -->
          <div class="card-mid">
            <div class="row">
              <div class="col-12 text-center temp">
                <span>{{ weather.temperature }}&deg;C</span>
                <p class="my-4">{{ weather.description }}</p>
              </div>
            </div>
            <div class="row">
              <div class="col d-flex justify-content-between px-5 mx-5">
                <p>
                  <!-- <img src="./assets/down.svg" alt="" /> -->
                  {{ weather.lowTemp }}&deg;C
                </p>
                <p>
                  <!-- <img src="./assets/up.svg" alt="" /> -->
                  {{ weather.highTemp }}&deg;C
                </p>
              </div>
            </div>
          </div>
          <!-- card middle ends here -->

          <!-- card bottom starts here -->
          <div class="card-bottom px-5 py-4 row">
            <div class="col text-center">
              <p>{{ weather.feelsLike }}&deg;C</p>
              <span>Feels like</span>
            </div>
            <div class="col text-center">
              <p>{{ weather.humidity }}%</p>
              <span>humidity</span>
            </div>
          </div>

          <!-- card bottom ends here -->
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
      cityName: "Gwarinpa",
      country: "NG",
      temperature: 12,
      description: "Clouds everywhere",
      lowTemp: "19",
      highTemp: "30",
      feelsLike: "20",
      humidity: "55",
    },
  }),
};
</script>

<style>
/* @import "./assets/custom.css"; */
</style>
