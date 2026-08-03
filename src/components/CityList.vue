<template>
  <div class="city-list">

    <div
      v-if="savedCities.length"
      class="cities-grid"
    >

      <CityCard
        v-for="city in savedCities"
        :key="city.id"
        :city="city"
        @click="goToCityView(city)"
      />

    </div>


    <div
      v-else
      class="empty"
    >
      <i class="fa-solid fa-location-dot"></i>

      <p>
        No locations added yet.
      </p>

      <span>
        Search for a city above to start tracking weather.
      </span>

    </div>


  </div>
</template>

<script setup>
import axios from 'axios'
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import CityCard from './CityCard.vue'

const savedCities = ref([])
const router = useRouter()

const fetchWeather = async (city) => {
  try {
    const { data } = await axios.get(
      "https://api.open-meteo.com/v1/forecast",
      {
        params: {
          latitude: city.coords.lat,
          longitude: city.coords.lng,
          current_weather: true,
        },
      }
    );

    return {
      temp: data.current_weather.temperature,
      time: data.current_weather.time,
    };
  } catch (err) {
    console.log(err);

    return {
      temp: 0,
      time: null,
    };
  }
};

const getCities = async () => {
  const stored = localStorage.getItem('savedCities')

  if (!stored) return

  const cities = JSON.parse(stored)

  savedCities.value = await Promise.all(
    cities.map(async (city) => {
      const weather = await fetchWeather(city)

      return {
        ...city,
        weather: weather || { temp: 'N/A', time: null },
      }
    })
  )
}

onMounted(getCities)

const goToCityView = (city) => {
  router.push({
    name: 'cityView',
    params: {
      state: city.state,
      city: city.city,
    },
    query: {
      id: city.id,
      lat: city.coords.lat,
      lng: city.coords.lng,
    },
  })
}
</script>

<style scoped>

.city-list{

width:100%;

}



.cities-grid{

display:grid;

grid-template-columns:
repeat(auto-fit,minmax(300px,1fr));

gap:20px;

padding:20px 0;

}



.empty{

margin-top:40px;

padding:40px;

text-align:center;

color:white;

background:
rgba(255,255,255,.08);

backdrop-filter:blur(12px);

border-radius:25px;

}


.empty i{

font-size:45px;

margin-bottom:20px;

color:#38bdf8;

}



.empty p{

font-size:22px;

font-weight:bold;

margin-bottom:10px;

}



.empty span{

opacity:.8;

}




@media(max-width:600px){

.cities-grid{

grid-template-columns:1fr;

}

}


</style>