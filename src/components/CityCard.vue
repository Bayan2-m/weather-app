<template>
  <div
    class="city-card"
  >

    <!-- City Info -->
    <div class="city-info">
      <h2>{{ city.city }}</h2>
      <p>{{ city.state }}</p>
    </div>


    <!-- Weather -->
    <div v-if="city.weather" class="weather-info">

      <div class="temp">
        {{ Math.round(city.weather.temp) }}°
      </div>


      <div class="details">

        <span v-if="city.weather.max">
          H: {{ Math.round(city.weather.max) }}°
        </span>

        <span v-if="city.weather.min">
          L: {{ Math.round(city.weather.min) }}°
        </span>

      </div>

    </div>


    <!-- Loading -->
    <div 
      v-else 
      class="loading"
    >
      Loading...
    </div>


  </div>
</template>


<script setup>

defineProps({
  city:{
    type:Object,
    required:true
  }
})

</script>



<style scoped>

.city-card{

  display:flex;
  justify-content:space-between;
  align-items:center;

  padding:25px;

  border-radius:25px;

  background:
  rgba(255,255,255,0.08);

  backdrop-filter:blur(12px);

  border:1px solid rgba(255,255,255,.15);

  color:white;

  cursor:pointer;

  transition:.3s ease;

}


.city-card:hover{

  transform:translateY(-5px);

  background:
  rgba(255,255,255,.15);

  box-shadow:
  0 15px 35px rgba(0,0,0,.25);

}



.city-info h2{

  font-size:28px;

  margin:0;

}


.city-info p{

  margin-top:5px;

  opacity:.8;

  font-size:16px;

}



.weather-info{

  text-align:right;

}



.temp{

  font-size:42px;

  font-weight:bold;

}



.details{

  display:flex;

  gap:12px;

  margin-top:8px;

  font-size:14px;

  opacity:.9;

}



.loading{

  opacity:.7;

  font-size:14px;

}




@media(max-width:600px){


.city-card{

  padding:20px;

}


.city-info h2{

  font-size:22px;

}


.temp{

  font-size:32px;

}


.details{

  flex-direction:column;

  gap:2px;

}


}


</style>