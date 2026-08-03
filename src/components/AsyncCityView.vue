<template>
  <div class="weather-page">

    <div v-if="loading" class="loading">
      Loading weather...
    </div>


    <div v-else-if="weatherData" class="weather-container">


      <!-- Preview Banner -->
      <div
        v-if="route.query.preview"
        class="preview"
      >
        You are currently previewing this city.
      </div>



      <!-- Current Weather Card -->

      <section class="current-card">

        <h1>
          {{ route.params.city }}
        </h1>

        <p class="time">
          {{ formatTime(weatherData.current.time) }}
        </p>


        <i
          class="weather-icon"
          :class="getWeatherIcon(weatherData.current.code)"
        ></i>


        <h2>
          {{ Math.round(weatherData.current.temp) }}°
        </h2>


        <p class="status">
          Current Weather
        </p>

      </section>



      <!-- Hourly -->

      <section class="section">

        <h2>
          Hourly Forecast
        </h2>


        <div class="hourly">

          <div
            v-for="(temp,i) in weatherData.hourly.temp"
            :key="i"
            class="hour-card"
          >

            <p>
              {{formatHour(weatherData.hourly.time[i])}}
            </p>


            <i
              :class="getWeatherIcon(weatherData.hourly.code[i])"
            ></i>


            <strong>
              {{Math.round(temp)}}°
            </strong>

          </div>


        </div>


      </section>




      <!-- Daily -->

      <section class="section">


        <h2>
          7 Day Forecast
        </h2>



        <div class="daily">


          <div
            v-for="(max,i) in weatherData.daily.max"
            :key="i"
            class="day-card"
          >

            <p>
              {{
              new Date(weatherData.daily.time[i])
              .toLocaleDateString(
              'en-GB',
              {weekday:'long'}
              )
              }}
            </p>



            <i
             :class="getWeatherIcon(weatherData.daily.code[i])"
            ></i>



            <div>

              <span>
                H:
                {{Math.round(max)}}
              </span>


              <span>
                L:
                {{Math.round(weatherData.daily.min[i])}}
              </span>

            </div>


          </div>


        </div>


      </section>



      <!-- Remove -->

      <button
      class="remove"
      @click="removeCity"
      >

      <i class="fa-solid fa-trash"></i>

      Remove City

      </button>


    </div>

  </div>
</template>

<script setup>
import axios from "axios"
import { useRoute, useRouter } from "vue-router"
import { ref, onMounted } from "vue"

const route = useRoute()
const router = useRouter()

const weatherData = ref(null)
const loading = ref(true)


const getWeatherIcon = (code) => {
  if (code === 0) return "fa-solid fa-sun"
  if (code >= 1 && code <= 3) return "fa-solid fa-cloud-sun"
  if (code >= 45 && code <= 48) return "fa-solid fa-smog"
  if (code >= 51 && code <= 57) return "fa-solid fa-cloud-rain"
  if (code >= 61 && code <= 67) return "fa-solid fa-cloud-showers-heavy"
  if (code >= 71 && code <= 77) return "fa-solid fa-snowflake"
  if (code >= 80 && code <= 82) return "fa-solid fa-cloud-showers-heavy"
  if (code >= 95) return "fa-solid fa-bolt"
  return "fa-solid fa-question"
}


const formatTime = (time) => {
  return new Date(time).toLocaleString("en-US", {
    timeZone: "Europe/Riga",
    hour: "2-digit",
    minute: "2-digit",
    hour12: true,
  })
}


const formatHour = (time) => {
  return new Date(time).toLocaleTimeString("en-US", {
    hour: "numeric",
    hour12: true,
  })
}


const getWeather = async () => {
  const { data } = await axios.get(
    "https://api.open-meteo.com/v1/forecast",
    {
      params: {
        latitude: route.query.lat,
        longitude: route.query.lng,
        hourly: "temperature_2m,weather_code",
        daily: "temperature_2m_max,temperature_2m_min,weather_code",
        current_weather: true,
        timezone: "auto",
      },
    }
  )

  return {
    current: {
      temp: data.current_weather.temperature,
      time: data.current_weather.time,
      code: data.current_weather.weathercode,
    },
    hourly: {
      time: data.hourly.time,
      temp: data.hourly.temperature_2m,
      code: data.hourly.weather_code,
    },
    daily: {
      time: data.daily.time,
      max: data.daily.temperature_2m_max,
      min: data.daily.temperature_2m_min,
      code: data.daily.weather_code,
    },
  }
}


onMounted(async () => {
  try {
    weatherData.value = await getWeather()
  } catch (err) {
    console.error("Weather fetch error:", err)
  } finally {
    loading.value = false
  }
})

/* 🗑️ REMOVE CITY (FIXED) */
const removeCity = () => {
  const cities = JSON.parse(localStorage.getItem("savedCities")) || []

  const updated = cities.filter(
    (c) => String(c.id) !== String(route.query.id)
  )

  localStorage.setItem("savedCities", JSON.stringify(updated))

  router.push({ name: "home" })
}
</script>

<style scoped>


.weather-page{

min-height:100vh;

padding:40px 20px;

display:flex;

justify-content:center;

background:
linear-gradient(
135deg,
#0369a1,
#0ea5e9
);

color:white;

}



.weather-container{
 width:100%;
 max-width:900px;
 padding:20px;
}


@media(max-width:600px){

.current-card{
 padding:25px;
}


.current-card h2{
 font-size:60px;
}


.hour-card{
 min-width:90px;
}


}



.loading{

font-size:25px;

}



.preview{

background:#facc15;

color:#111;

padding:15px;

border-radius:15px;

text-align:center;

margin-bottom:20px;

}




.current-card{

text-align:center;

padding:40px;

border-radius:30px;

background:
rgba(255,255,255,.12);

backdrop-filter:blur(15px);

box-shadow:
0 20px 40px rgba(0,0,0,.3);

}



.current-card h1{

font-size:40px;

}



.time{

opacity:.8;

}



.weather-icon{

font-size:80px;

margin:25px;

}



.current-card h2{

font-size:90px;

margin:0;

}



.status{

font-size:20px;

opacity:.8;

}




.section{

margin-top:40px;

}



.section h2{

margin-bottom:20px;

}




.hourly{

display:flex;

gap:15px;

overflow-x:auto;

}



.hour-card{

min-width:100px;

padding:20px;

border-radius:20px;

text-align:center;

background:
rgba(255,255,255,.12);

}

.hour-card:hover{
 transform:translateY(-8px);
 background:rgba(255,255,255,.2);
}



.hour-card i{

font-size:35px;

margin:15px;

}




.daily{

display:flex;

flex-direction:column;

gap:15px;

}



.day-card{


display:flex;

align-items:center;

justify-content:space-between;


padding:20px;

border-radius:20px;


background:
rgba(255,255,255,.12);


}



.day-card i{

font-size:35px;

}



.day-card span{

margin-left:15px;

}




.remove{


margin:40px auto 0;

display:block;


padding:15px 30px;

border:none;

border-radius:15px;


background:#ef4444;

color:white;


font-size:18px;

cursor:pointer;


}




.remove:hover{

transform:scale(1.05);

}




@media(max-width:600px){


.current-card h1{

font-size:30px;

}


.current-card h2{

font-size:65px;

}



.day-card{

font-size:14px;

padding:15px;

}


.weather-icon{
  animation: float 3s infinite ease-in-out;
}


@keyframes float{

0%,100%{
 transform:translateY(0);
}

50%{
 transform:translateY(-10px);
}

}


}



</style>