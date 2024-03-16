<script setup>
import { ref } from 'vue';
import borderLine from './borderLine.vue';
import forecastWeek from './forecastWeek.vue';
import moreInfo from './moreInfo.vue';

defineProps({
  place: Object
});

const emit = defineEmits(['delete-city']);
const showDetails = ref(false);

const deleteCard = (placeName) => {
  emit('delete-city', placeName);
  showDetails.value = false;
};

const prevSlide = () => {
  const wrapper = document.querySelector('.hourly-weather-carousel');
  wrapper.scrollLeft -= wrapper.clientWidth;
};

const nextSlide = () => {
  const wrapper = document.querySelector('.hourly-weather-carousel');
  wrapper.scrollLeft += wrapper.clientWidth;
};

</script>



<template>
  <div :class="[place && place.current && place.current.is_day === 1 ? 'bg-day' : 'bg-night', 'text-white p-4 rounded-lg shadow-lg gap-4 mb-4 relative overflow-hidden']">
    <!-- Location & time -->
    <div class="mb-2">
      <div class="flex items-center justify-end mb-2">
        <button @click="$emit('delete-city', place.location.name)">
          <i class="fa-solid fa-trash hover:text-[#F2B705]"></i>
        </button>
      </div>

      <div>
        <div class="flex items-center justify-start">
          <div class="flex items-center gap-2">
            <i class="fa-solid fa-location-dot text-[#24ceff]"></i>
            <h1 class="text-3xl">{{ place.location.name }}</h1>
          </div>
        </div>

        <div class="flex items-center justify-start">
          <div class="flex items-center gap-2">
            <i class="fa-solid fa-clock text-[#24ceff]"></i>
            <h1 class="text-2xl">{{ String(new Date(place.location.localtime).getHours()).padStart(2, '0') }}:{{ String(new Date(place.location.localtime).getMinutes()).padStart(2, '0') }}</h1>
          </div>
        </div>
      </div>
    </div>

    <!-- current weather -->
    <div class="text-center flex-1">
      <img :src="place.current.condition.icon" alt="icon" width="100" class="mx-auto -mb-13" />
      <h1 class="text-4xl mb-4">{{ Math.round(place.current.temp_c) }}&deg;</h1>
      <p class="text-2xl">{{ place.current.condition.text }}</p>
    </div>

    <borderLine />
    
    <!-- hourly weather -->
    <div class="hourly-weather-carousel">
      <div class="hourly-weather-wrapper">
        <div class="hourly-weather-slide" v-for="(hour, index) in place.forecast.forecastday[0].hour" :key="index">
          <div class="hourly-weather-item">
            <p>{{ new Date(hour.time).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }) }}</p>
            <img :src="hour.condition.icon" alt="icon" width="30">
            <p>{{ Math.round(hour.temp_c) }}°C</p>
          </div>
        </div>
      </div>
      <button class="carousel-prev" @click="prevSlide"><i class="fa-solid fa-circle-chevron-left"></i></button>
      <button class="carousel-next" @click="nextSlide"><i class="fa-solid fa-circle-chevron-right"></i></button>
    </div>

    <borderLine />

    <!-- forecast -->
    <div v-if="place.forecast && place.forecast.forecastday" v-for="(day, idx) in place.forecast.forecastday" :key="idx">
      <forecastWeek :day="day"/>
    </div>

    <!-- More Details about the current weatehr -->
    <Transition name="fade">
      <div v-show="showDetails">
        <moreInfo :place="place" @close-info="showDetails = false" @delete-card="deleteCard(place.location.name)"/>
      </div>
    </Transition>

    <!-- forecast btn -->
    <div class="flex justify-end items-center gap-1 mt-10">
      <button class=" bg-[#F2B705] hover:bg-[#eeca5d] text-[#093358] font-bold py-2 px-4 rounded " @click="showDetails = true">Details</button>
    </div>
  </div>
</template>

<style scoped>
  .bg-day {
    background-color: #D0E7FF;
    background-image: linear-gradient(62deg, #1f6cb4 0%, #9ec9f1 100%);
  }
  .bg-night {
    background-color: #07223d;
    background-image: linear-gradient(62deg, #0a2a4a 0%, #270845 100%);
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.2s ease;
  }

  .fade-enter-from,
  .fade-leave-to { 
    opacity: 0;
  }

  .hourly-weather-carousel {
    overflow: hidden;
  }

  .hourly-weather-wrapper {
    display: flex;
    position: relative;
   
  }

  .hourly-weather-slide {
    flex: 0 0 auto;
    margin-right: 10px; 
    transition: transform 0.5s ease;
  }

  .carousel-prev,
  .carousel-next {
    position:absolute;
    top: 50%;
    background-color: #07223d33;
    color: white;
    border: none;
    padding: 3px 5px;
    cursor: pointer;
  }

  .carousel-prev {
    left: 10px;
  }

  .carousel-next {
    right: 10px;
  }
</style>
