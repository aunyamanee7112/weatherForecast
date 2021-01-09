<template>
  <main>
    <div class="currentCity">
      <div
        class="mainData"
        :style="
          weather.temperature < 15
            ? { backgroundImage: 'url(' + require('@/assets/winter.jpg') + ')' }
            : {
                backgroundImage: 'url(' + require('@/assets/summer2.jpg') + ')',
                color: 'white',
              }
        "
      >
        <!-- <h1 v-if="cityFound">Not Found Your City</h1> -->
        <p>Today</p>
        <p>{{ dt }}</p>
        <h1 style="font-size:60px">{{ weather.temperature }}&deg;C</h1>
        <h1>{{ weather.cityname }}</h1>

        <h5 style="font-size:20px">{{ weather.description }}</h5>
        <br />
      </div>
    </div>
    <div class="weatherForecast">
      <div>
        <div class="weatherTap">
          <h3>Weather Forcast</h3>
          <form
            class="form-control text-muted form-rounded p-4 shadow-sm"
            v-on:submit.prevent="getWeather"
          >
            <input
              type="text"
              placeholder="what's city"
              autocomplete="off"
              v-model="citySearch"
            />
          </form>
          <div>
            <img
              :src="require('../assets/high.png')"
              width="30"
              style="margin-right:5px"
            />
            <h5>Max</h5>
            <p>{{ weather.highTemp }}&deg;C</p>
          </div>
          <div>
              <img
                :src="require('../assets/low.png')"
                width="30"
                style="margin-right:5px"
              />
              <h5>Min</h5>
              <p>{{ weather.lowTemp }}&deg;C</p>
          </div>
          <div style="width:300px">
              <img
                :src="require('../assets/cloud.png')"
                width="30"
                style="margin-right:5px"
              />
              <h5>Feel Like</h5>
              <p>{{ weather.feelsLike }}&deg;C</p>
          </div>
        </div>

        <div class="forecast">
          <div
            class="forecastBox"
            v-for="(result, index) in weatherForecast"
            v-bind:key="index"
          >
            <h4>{{ days[index] }}</h4>
            <img :src="require(`../assets/${img}`)" width="50" />
            <h3>{{ (result.temp.day - 273.15).toFixed(0) }}&deg;</h3>
          </div>
        </div>
        <br />
        <div class="gridBox">
          <h3 class="textTopic">Today's highLights</h3>
          <div class="highLight">
            <div class="box">
              <div class="detail">
                <h4>Uv Index</h4>
                <div class="progressBar">
                  <radial-progress-bar
                    :diameter="140"
                    :completed-steps="uv"
                    :total-steps="totalSteps"
                    innerStrokeColor="#E4E6EB"
                    startColor="#FFBC60"
                    stopColor="#FFBC60"
                  >
                    <h1>{{ uv.toFixed(1) }}</h1>
                  </radial-progress-bar>
                </div>
              </div>
            </div>
            <div class="box">
              <div class="detail details">
                <h4>Wind Status</h4>
                <h1 style="font-size:40px">{{ weather.wind }} Km/h</h1>
                <span>
                  <img :src="require(`../assets/compass4.png`)" width="30" />

                  <h4 style="margin-left:10px ; color:#2c3e50" v-if="deg !== 0">
                    {{ deg }}
                  </h4>
                </span>
              </div>
            </div>
            <div class="box">
              <div class="detail details">
                <h4>Air Polution (pm 2.5)</h4>
                <h1>{{ pm2_5 }}</h1>
                <p style="font-family: 'Chonburi', cursive; font-size:20px">
                  {{ pmResult }}
                </p>
              </div>
            </div>
            <div class="box">
              <div class="detail">
                <h4>Pressure</h4>
                <br />
                <h1>{{ weather.pressure }} hPa</h1>
              </div>
            </div>
            <div class="box">
              <div class="detail">
                <h4>Humid</h4>
                <br />
                <div>
                  <h1>{{ weather.humid }}%</h1>
                  <img
                    :src="require('../assets/008-humidity.png')"
                    width="70"
                  />
                </div>
              </div>
            </div>
            <div class="box">
              <div class="detail sunTime">
                <h4>Sunrise & Sunset</h4>
                <div>
                  <img :src="require('@/assets/024-sunrise.png')" width="50" />
                  <span
                    >{{ weather.sunrise }}<br />
                    <p
                      style="font-family: 'Chonburi', cursive; font-size:10px; color:gray"
                    >
                      พระอาทิตย์ขึ้น
                    </p></span
                  >
                </div>
                <div>
                  <img :src="require('@/assets/025-sunset.png')" width="50" />
                  <span
                    >{{ weather.sunset }}<br />
                    <p
                      style="font-family: 'Chonburi', cursive; font-size:10px; color:gray"
                    >
                      พระอาทิตย์ตก
                    </p></span
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import RadialProgressBar from "vue-radial-progress";

export default {
  name: "HelloWorld",
  //props กำหนดว่า component นี้รับค่าอะไรได้บ้าง
  props: {
    msg: String,
  },
  data() {
    return {
      totalSteps: 20,
      bg: "../assets/open.png",
      dt: new Date().toDateString(),
      img: "002-cloudy.png",
      cityFound: false,
      isDay: true,
      citySearch: "Thailand",
      key: "e363f0feb1633bb85b4faf9c01582f62",
      myChart: [],
      weather: {
        cityname: "Thailand",
        temperature: 29,
        description: "Clear Sky ",
        lowTemp: 22,
        highTemp: 30,
        feelsLike: 30,
        humid: 0,
        pressure: 0,
        wind: 0,
        degreeWind: 0,
        sunset: 0,
        sunrise: 0,
      },
      test: [1, 2, 3, 4],
      uv: 0,
      pm2_5: 0,
      pmResult: "",
      weatherForecast: [],
      days: ["Sun", "Mon", "Tue", "Wed", "Thur", "Fri", "Sat", "Sun"],
      deg: "",
    };
  },
  components: {
    RadialProgressBar,
  },
  mounted() {
    this.getWeather();
  },
  methods: {
    hi: function() {
      console.log("hi");
    },

    getUvIndex: async function(lat, lon) {
      const uvIndex = `http://api.openweathermap.org/data/2.5/uvi?lat=${lat}&lon=${lon}&appid=${this.key}`;
      const responde = await fetch(uvIndex);
      const data = await responde.json();
      // console.log('uvindex:',data);
      this.uv = data.value;
    },
    getAirPolution: async function(lat, lon) {
      const airPolution = `http://api.openweathermap.org/data/2.5/air_pollution?lat=${lat}&lon=${lon}&appid=${this.key}`;
      const responde = await fetch(airPolution);
      const data = await responde.json();
      //  console.log('pm 2.5 : ' ,data.list[0].components.pm2_5);
      this.pm2_5 = data.list[0].components.pm2_5;
      if (this.pm2_5 >= 0 && this.pm2_5 < 25) {
        this.pmResult = "ดีมาก";
      } else if (this.pm2_5 >= 26 && this.pm2_5 < 50) {
        this.pmResult = "ดี";
      } else if (this.pm2_5 >= 51 && this.pm2_5 < 100) {
        this.pmResult = "ปานกลาง";
      } else if (this.pm2_5 >= 100 && this.pm2_5 < 200) {
        this.pmResult = "เริ่มมีผลต่อสุขภาพ";
      } else {
        this.pmResult = "มีผลกระทบต่อสุขภาพ";
      }
    },
    getWeatherForcast: async function(lat, lon) {
      const weekForecast = `https://api.openweathermap.org/data/2.5/onecall?lat=${lat}&lon=${lon}&exclude=minutely,current,hourly,alerts&appid=${this.key}`;
      const responde = await fetch(weekForecast);
      const data = await responde.json();
      this.weatherForecast = data.daily;
      for (let index = 0; index < 8; index++) {
        await console.log(
          "7days forecast:",
          data.daily[index].weather[0].description
        );
        const description = data.daily[index].weather[0].description;

        if (description === "clear sky") {
          this.img = "001-sunny.png";
        } else if (description === "few clouds") {
          this.img = "001-sunny.png";
        } else if (description === "scattered clouds") {
          this.img = "002-cloudy.png";
        } else if (description === "	broken clouds") {
          this.img = "002-cloudy.png";
        } else if (description === "	overcast clouds") {
          this.img = "010-wind.png";
        } else if (description === "shower rain") {
          this.img = "005-heavy rain.png";
        } else if (description === "rain") {
          this.img = "003-rain.png";
        } else if (description === "thunderstorm") {
          this.img = "007-thunder.png";
        } else if (description === "snow" || description === "light snow") {
          this.img = "008-snow.png";
        } else if (description === "mist") {
          this.img = "009-snow.png";
        }
      }
    },
    getSunrise: async function(lat, lon) {
      const sunTime = `https://api.sunrise-sunset.org/json?lat=${lat}&lng=${lon}`;
      const responde = await fetch(sunTime);
      const data = await responde.json();
      // console.log("sunset  :", data.results.sunset);
      this.weather.sunrise = data.results.sunrise;
      this.weather.sunset = data.results.sunset;
    },
    getWeather: async function() {
      const baseURL = `http://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${this.key}`;
      try {
        this.cityFound = false;
        const responde = await fetch(baseURL);
        const data = await responde.json();
        // console.log("weather current: ", data);
        const lat = data.coord.lat;
        const lon = data.coord.lon;
        this.getUvIndex(lat, lon);
        this.getAirPolution(lat, lon);
        this.getWeatherForcast(lat, lon);
        this.getSunrise(lat, lon);
        const nameCity = this.citySearch;
        this.citySearch = "";
        this.weather.cityname = nameCity;
        this.weather.temperature = (data.main.temp - 273.15).toFixed(0);
        this.weather.lowTemp = (data.main.temp_min - 273.15).toFixed(0);
        this.weather.highTemp = (data.main.temp_max - 273.15).toFixed(0);
        this.weather.feelsLike = (data.main.feels_like - 273.15).toFixed(0);
        this.weather.humid = data.main.humidity;
        this.weather.pressure = data.main.pressure;
        this.weather.description = data.weather[0].description;
        this.weather.wind = data.wind.speed;
        this.weather.degreeWind = data.wind.deg;
        const TimeOfDay = data.weather[0].icon;
        this.dt = new Date().toDateString();
        //day & night
        if (TimeOfDay.includes("n")) {
          this.isDay = false;
        } else {
          this.isDay = true;
        }
        // deg wind
        if (
          this.weather.degreeWind >= 348.75 &&
          this.weather.degreeWind < 11.25
        ) {
          this.deg = "N";
        } else if (
          this.weather.degreeWind >= 11.25 &&
          this.weather.degreeWind < 33.75
        ) {
          this.deg = "NNE";
        } else if (
          this.weather.degreeWind >= 56.25 &&
          this.weather.degreeWind < 78.75
        ) {
          this.deg = "NE";
        } else if (
          this.weather.degreeWind >= 56.25 &&
          this.weather.degreeWind < 78.75
        ) {
          this.deg = "ENE";
        } else if (
          this.weather.degreeWind >= 78.75 &&
          this.weather.degreeWind < 101.25
        ) {
          this.deg = "E";
        } else if (
          this.weather.degreeWind >= 101.25 &&
          this.weather.degreeWind < 123.75
        ) {
          this.deg = "ESE";
        } else if (
          this.weather.degreeWind >= 168.75 &&
          this.weather.degreeWind < 191.25
        ) {
          this.deg = "SE";
        } else if (
          this.weather.degreeWind >= 123.75 &&
          this.weather.degreeWind < 146.75
        ) {
          this.deg = "SSE";
        } else if (
          this.weather.degreeWind >= 168.75 &&
          this.weather.degreeWind < 191.25
        ) {
          this.deg = "S";
        } else if (
          this.weather.degreeWind >= 191.25 &&
          this.weather.degreeWind < 213.75
        ) {
          this.deg = "SSW";
        } else if (
          this.weather.degreeWind >= 213.75 &&
          this.weather.degreeWind < 236.25
        ) {
          this.deg = "SW";
        } else if (
          this.weather.degreeWind >= 236.25 &&
          this.weather.degreeWind < 258.75
        ) {
          this.deg = "WSW";
        } else if (
          this.weather.degreeWind >= 258.75 &&
          this.weather.degreeWind < 281.25
        ) {
          this.deg = "W";
        } else if (
          this.weather.degreeWind >= 281.25 &&
          this.weather.degreeWind < 303.75
        ) {
          this.deg = "WNW";
        } else if (
          this.weather.degreeWind >= 303.75 &&
          this.weather.degreeWind < 326.25
        ) {
          this.deg = "NW";
        } else if (
          this.weather.degreeWind >= 326.25 &&
          this.weather.degreeWind < 348.75
        ) {
          this.deg = "NNW";
        }
      } catch (error) {
        console.log(error);
        this.cityFound = true;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Chonburi&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300&display=swap");
template {
  margin: 0 !important;
  padding: 0 !important;
}
input {
  padding: 5px;
  border: none;
  background: none;
  border-bottom: 1px solid rgb(182, 182, 182);
}
.progressBar {
  top: 5;
  height: 90px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.hello {
  /* background: rgb(235, 196, 196); */
  width: 100%;
  display: flex;
  overflow-y: hidden;
  flex-direction: column;
}
.hi {
  background: white;
  width: 100%;
  height: auto;
}
.highLight {
  display: grid;
  grid-template-columns: auto auto auto;
  grid-template-rows: 190px 190px 190px;
  column-gap: 15px;
  row-gap: 15px;
  overflow-y: hidden;
  width: auto;
  height: 60vh;
}
.box {
  width: auto;
  height: 190px;
  background-color: white;
  margin-top: 1em;
  border-radius: 20px;
  text-align: left;
}
.box .detail h4 {
  color: #bbbaba;
}
.box .detail h1 {
  font-size: 45px;
}
.box .detail div {
  display: flex;
  align-items: center;
  /* justify-content: space-around; */
  margin-top: 10px;
  /* font-family: 'Poppins', sans-serif; */
  font-size: 15px;
  font-weight: 600;
}
.box .detail div span {
  margin-left: 20px;
}

.box .details {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 120px;
}
.box .details span {
  display: flex;
  align-items: center;
}
main {
  display: flex;
  flex-direction: row;
  width: 100%;
  overflow: hidden;
}
.currentCity {
  width: 500px !important;
  height: auto;
  background: white;
  display: flex;
  flex-direction: column;
}
.weatherForecast {
  width: 100%;
  min-height: 100vh;
  height: auto;
  overflow: hidden;
  background-color: #f6f6f8;
  padding: 2em 3em 0 3em;
  margin-right: 0;
}
.textTopic {
  text-align: left;
}
.weatherTap {
  max-width: 100%;
  width: auto;
  display: flex;
  align-items: center;
  text-align: left;
}
.weatherTap h4 {
  padding: 5px;
  cursor: pointer;
}
.weatherTap h4:hover {
  text-decoration-line: underline;
  text-decoration-style: wavy;
}
.weatherTap input {
  margin-left: 10px;
  margin-right: 10px;
  width: 600px;
}
.weatherTap div  {
  width: 200px;
  display: flex;
  align-items: center;
  margin-left: 15px;
  justify-content: space-around;
}
.changeTemp {
  margin-left: 2em;
}
.changeTemp .tempBtn {
  cursor: pointer;
  padding: 5px;
  margin-left: 1em;
  background: rgb(255, 255, 255);
  border-radius: 100%;
  font-weight: 600;
}
.mainData {
  height: 100%;
  padding: 3em;
  background-image: url("../assets/summer2.jpg");
  background-size: cover;
  background-position: center;
  color: rgb(58, 65, 105);
  text-align: center;
  overflow: hidden;
}

.description {
  padding: 2em;
  font-family: "Poppins", sans-serif;
  text-align: left;
  background-image: linear-gradient(
    to bottom,
    rgba(255, 255, 255, 0.582),
    white
  );
  width: auto;
  z-index: 10;
}
.description div {
  display: flex;
  justify-content: space-between;
  margin-top: 0.5em;
}
.forecast {
  display: flex;
  justify-content: space-between;
  padding: 2em 0 1em 0;
}
.forecastBox {
  padding: 20px;
  width: 80px;
  height: 90px;
  background: white;
  border-radius: 25px;
}
.detail {
  padding: 2em;
}
</style>
