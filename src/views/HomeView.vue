<template>
  <main class="home-container">


    <!-- SEARCH -->
    <div class="search-box">


      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Search for a city..."
      />


      <!-- RESULTS -->

      <ul
        v-if="mapboxSearchResults !== null"
        class="results"
      >


        <p v-if="searchError">
          Sorry, something went wrong.
        </p>



        <p
          v-if="
          !searchError &&
          mapboxSearchResults &&
          mapboxSearchResults.length === 0
          "
        >
          No results found.
        </p>



        <li
          v-for="searchResult in mapboxSearchResults"
          :key="searchResult.id"
          @click="previewCity(searchResult)"
        >

          <i class="fa-solid fa-location-dot"></i>

          {{ searchResult.place_name }}

        </li>


      </ul>


    </div>



    <!-- CITIES -->

    <div class="cities">


      <Suspense>

        <CityList />


        <template #fallback>

          <CityCardSkeleton />

        </template>


      </Suspense>


    </div>


  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

import CityCardSkeleton from "../components/CityCardSkeleton.vue";
import CityList from "../components/CityList.vue";

const router = useRouter();

/* -------------------------
   SEARCH STATE
------------------------- */
const searchQuery = ref("");
const queryTimeout = ref(null);
const mapboxSearchResults = ref(null);
const searchError = ref(false);

/* -------------------------
   MAPBOX API KEY
------------------------- */

const mapboxAPIKey = import.meta.env.VITE_MAPBOX_TOKEN
/* -------------------------
   PREVIEW CITY
------------------------- */
const previewCity = (searchResult) => {
  const [city, state] = searchResult.place_name.split(",");

  router.push({
    name: "cityView",
    params: {
      city: city.trim(),
      state: state ? state.trim() : "",
    },
    query: {
      lat: searchResult.geometry.coordinates[1],
      lng: searchResult.geometry.coordinates[0],
      preview: true,
    },
  });
};
/* -------------------------
   SEARCH FUNCTION (DEBOUNCE)
------------------------- */
const getSearchResults = () => {
  clearTimeout(queryTimeout.value);

  queryTimeout.value = setTimeout(async () => {
    searchError.value = false;

    if (searchQuery.value.trim() !== "") {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json`,
          {
            params: {
              access_token: mapboxAPIKey,
              types: "place",
            },
          }
        );

        mapboxSearchResults.value = result.data.features;
      } catch (err) {
        searchError.value = true;
        mapboxSearchResults.value = [];
      }

      return;
    }

    mapboxSearchResults.value = [];
  }, 300);
};
</script>

<style scoped>

.home-container{

min-height:100vh;

padding:40px 20px;

max-width:1000px;

margin:auto;

}



/* Search */

.search-box{

position:relative;

margin-bottom:40px;

}



.search-box input{


width:100%;

padding:18px 20px;


border-radius:20px;

border:none;

outline:none;


font-size:18px;


background:
rgba(255,255,255,.12);


backdrop-filter:blur(12px);


color:white;


border:1px solid rgba(255,255,255,.2);


}



.search-box input::placeholder{

color:#e5e7eb;

}




/* Results */

.results{


position:absolute;


top:65px;


width:100%;


background:

rgba(15,23,42,.85);


backdrop-filter:blur(15px);


border-radius:20px;


padding:10px;


list-style:none;


z-index:10;


box-shadow:

0 15px 30px rgba(0,0,0,.3);


}




.results li{


padding:15px;


border-radius:12px;


cursor:pointer;


transition:.3s;


color:white;


}



.results li:hover{


background:

rgba(255,255,255,.15);


transform:translateX(5px);


}



.results i{

margin-right:10px;

color:#38bdf8;

}




.results p{

padding:15px;

color:white;

}





.cities{

margin-top:20px;

}




@media(max-width:600px){


.home-container{

padding:25px 15px;

}


.search-box input{

font-size:16px;

}



}

</style>