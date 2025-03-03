<template>
  <main
    id="app"
    :style="{
      backgroundImage: `linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75)), ${backgroundImage}`,
    }"
  >
    <div class="search-box">
      <input
        type="text"
        placeholder="Search..."
        class="search-bar"
        v-model="query"
        @keypress="fetchWeather"
      />
    </div>
    <!-- {{ query }} -->
    <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
      <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
      <div class="date">{{ dataBuilder() }}</div>
      <div class="weather-wrapper">
        <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
        <div class="weather">{{ weather.weather[0].description }}</div>

        <div class="extra-info">
          Humidity: {{ weather.main.humidity }} <br />
          Wind Speed: {{ weather.wind.speed }} <br />
          Pressure: {{ weather.main.pressure }}
        </div>
      </div>
    </div>
  </main>
</template>

<script >
export default {
  name: "app",
  data() {
    return {
      api_key: "aaa3fb59df975a5520ce297d2a81fadd",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key === "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      console.log(results);
    },
    dataBuilder() {
      let d = new Date();
      let months = [
        "January",
        "Feb",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },

  computed: {
  backgroundImage() {
    if (!this.weather.weather) return "";

    const description = this.weather.weather[0].description.toLowerCase();
    let imagePath;

    if (description.includes("clear")) {
      imagePath = new URL('@/assets/clear-bg.jpg', import.meta.url).href;
    } else if (description.includes("cold")) {
      imagePath = new URL('@/assets/cold-bg.jpg', import.meta.url).href;
    }  else if (description.includes("cloud")) {
      imagePath = new URL('@/assets/cloud-bg.jpg', import.meta.url).href;
    } 
    else if (description.includes("rain")) {
      imagePath = new URL('@/assets/rain-bg.jpg', import.meta.url).href;
    } 
    else if (description.includes("warm")) {
      imagePath = new URL('@/assets/warm-bg.jpg', import.meta.url).href;
    } else if (description.includes("haze")) {
      imagePath = new URL('@/assets/haze-bg.jpg', import.meta.url).href;
    } else {
      imagePath = new URL('@/assets/default-bg.jpg', import.meta.url).href;
    }

    console.log("Background image path:", imagePath);
    return `linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.50)), url(${imagePath})`;
  },
},

};
</script>

<style >
* {
  margin: 0;
  border: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}

#app {
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  padding: 25px;
}
.search-box {
  width: 100%;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  color: #313131;
  font-size: 20px;
  background-color: rgba(225, 225, 225, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(225, 225, 225, 0.75);
  border-radius: 16px 0px 16px 0px;
  border: none;
  outline: none;
}
.location {
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px grey;
  padding-top: 40px;
}
.date {
  color: white;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}
.weather-wrapper {
  text-align: center;
}
.weather-wrapper .temp {
  display: inline-block;
  padding: 10px 15px;
  color: white;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgb(82, 81, 81);
  background-color: rgba(0, 0, 0, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgb(89, 87, 87);
}
.weather {
  color: white;
  font-size: 48px;
  font-weight: 700;
  text-align: center;
  text-shadow: 2px 2px grey;
}
.extra-info {
  display: inline-block;
  padding: 10px 15px;
  color: white;
  font-size: 20px;
  font-weight: 30;
  background-color: rgba(0, 0, 0, 0.25);
  margin: 30px 0px;
  text-align: left;
  font-style: italic;
}
</style>
