<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Search"
        v-model="query"
        @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°F</div>
          <div class="feelslike">Feels Like {{ Math.round(weather.main.feels_like)}}°F</div>
          <div class="weather">{{weather.weather[0].main}}</div>
          <div class="wind">{{Math.round(weather.wind.speed)}} mph {{windDirection(weather.wind.deg)}}</div>
          <div class="humidity">{{weather.main.humidity}}%</div>
          <div class="pressure">{{weather.main.pressure}}hPa</div>
          <div class="visibility">{{weather.visibility/1000}}km</div>
          <div class="sunrise">Sunrise: {{Unix_timestamp(weather.sys.sunrise)}}</div>
          <div class="sunset">Sunset: {{Unix_timestamp(weather.sys.sunset)}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      api_key: 'c40144b1a3d5ef1e331b1edbc533a817',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=imperial&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results){
      this.weather = results;
    },
    dateBuilder (){
      let d = new Date();
      let months = ["January","February","March","April","May","June","July","August","September","October","November","December"];
      let days = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
    windDirection(dir){
      if(dir <= 45 || dir > 315){
        return "N";
      }
      if(dir > 45 && dir <= 135){
        return "E";
      }
      if(dir > 135 && dir <= 225){
        return "S";
      }
      if(dir > 225 && dir <= 315){
        return "W";
      }
    },
    Unix_timestamp(t){
      let dt = new Date(t*1000);
      let hr = dt.getHours();
      let m = "0" + dt.getMinutes();
      let s = "0" + dt.getSeconds();
      return hr+ ':' + m.substr(-2) + ':' + s.substr(-2);  
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'montserrat', sans-serif;
}

#app{
  background-image: url('https://image.freepik.com/free-photo/clear-blue-sky-with-white-cloud-background-clearing-day-good-weather-morning_53476-2596.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.5s; 
}

main{
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;
  
  appearance: none;
  border:none;
  outline:none;
  background:none;

  box-shadow: 20px 20px 20px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 40px 40px 40px 40px;
  transition: 0.7s;
}

.search-box .search-bar:focus{
  box-shadow: 40px 40px 40px rgba(0,0,0,0.25);
  background-color:rgba(255,255,255,0.75);
  border-radius: 40px 40px 40px 40px;
}

.location-box{
  text-align:center;
}

.location-box .location{
  color: #FFF;
  font-size: 66px;
  font-weight: 600;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}

.location-box .date{
  color:#FFF;
  font-size: 24px;
  font-weight: 100;
  font-style: italic;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.weather-box{
  text-align:center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color:aquamarine;
  font-size:102px;
  font-weight:900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .feelslike{
  font-style:italic;
  padding: 10px 25px;
  color:aquamarine;
  font-size:42px;
  font-weight:900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather{
  color:#FFF;
  font-size:48px;
  font-weight: 700;
  font-style:normal;
  text-shadow:3px 6px rgba(0,0,0,0.25);
}

.weather-box .wind{
  color:#FFF;
  font-size: 48px;
  font-weight: 700;
  text-shadow:3px 6px rgba(0,0,0,0.25);
}

.weather-box .humidity{
  color:#FFF;
  font-size: 36px;
  font-weight: 700;
  text-shadow:2px 4px rgba(0,0,0,0.33);
}

.weather-box .pressure{
  color:#FFF;
  font-size: 36px;
  font-weight: 700;
  text-shadow:2px 4px rgba(0,0,0,0.33);
}

.weather-box .visibility{
  color:#FFF;
  font-size: 36px;
  font-weight: 700;
  text-shadow:2px 4px rgba(0,0,0,0.33);
}

.weather-box .sunrise{
  display:inline-block;
  padding:10px 25px;
  color:#FFF;
  font-size: 36px;
  font-weight: 700;
  text-shadow:2px 4px rgba(0,0,0,0.33);
}

.weather-box .sunset{
  display:inline-block;
  padding:10px 25px;
  color:#FFF;
  font-size: 36px;
  font-weight: 700;
  text-shadow:2px 4px rgba(0,0,0,0.33);
}
</style>
