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
  <div :class="[place && place.current && place.current.is_day === 1 ? 'bg-day' : 'bg-night', 'text-white p-3 md:p-2 rounded-lg shadow-lg gap-4 mb-4 relative overflow-hidden']">
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
                    <i class="fa-solid fa-location-dot text-[#F2B705]"></i>
                    <h1 class="text-lg md:text-xl">{{ place.location.name }}</h1>
                </div>
            </div>

            <div class="flex items-center justify-start">
                <div class="flex items-center gap-2">
                    <i class="fa-solid fa-clock text-[#F2B705]"></i>
                    <h1 class="text-lg md:text-xl">{{ String(new Date(place.location.localtime).getHours()).padStart(2, '0') }}:{{ String(new Date(place.location.localtime).getMinutes()).padStart(2, '0') }}</h1>
                </div>
            </div>
        </div>
    </div>

    <!-- current weather -->
    <div class="text-center flex-1 sm:p2">
      <img :src="place.current.condition.icon" alt="icon" width="100" class="mx-auto mb-2 md:mb-4 md:w-20 lg:w-30" />
      <h1 class="text-3xl md:text-2xl lg:text-5xl mb-2">{{ Math.round(place.current.temp_c) }}&deg;</h1>
      <p class="text-lg md:text-xl">{{ place.current.condition.text }}</p>
    </div>

    <borderLine />

    <!-- hourly weather -->
    <div class="hourly-weather-carousel">
        <div class="hourly-weather-wrapper">
            <div class="hourly-weather-slide" v-for="(hour, index) in place.forecast.forecastday[0].hour" :key="index">
                <div class="hourly-weather-item bg-[#ffffff62] px-5 py-3 rounded-lg">
                    <p>{{ new Date(hour.time).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }) }}</p>
                    <img :src="hour.condition.icon" alt="icon" width="30">
                    <p>{{ Math.round(hour.temp_c) }}°C</p>
                </div>
            </div>
        </div>
        <button class="carousel-prev md:hidden" @click="prevSlide"><i class="fa-solid fa-circle-chevron-left"></i></button>
        <button class="carousel-next md:hidden" @click="nextSlide"><i class="fa-solid fa-circle-chevron-right"></i></button>
    </div>

    <borderLine />

    <!-- forecast -->
    <div class="bg-[#ffffff2a] p-4 rounded-lg">
        <div v-if="place.forecast && place.forecast.forecastday" v-for="(day, idx) in place.forecast.forecastday" :key="idx">
            <forecastWeek class="font-bold" :day="day"/>
        </div>
    </div>

    <!-- More Details about the current weather -->
    <Transition name="fade">
        <div v-show="showDetails">
            <moreInfo :place="place" @close-info="showDetails = false" @delete-card="deleteCard(place.location.name)"/>
        </div>
    </Transition>

    <!-- forecast btn -->
    <div class="flex justify-end items-center gap-1 mt-10">
        <button class="bg-[#F2B705] hover:bg-[#eeca5d] text-[#093358] font-bold py-2 px-4 rounded md:text-sm lg:text-base" @click="showDetails = true">Details</button>
    </div>
  </div>

</template>

<style scoped>
  .bg-day {
    background-image: url(../assets/day.png);
    background-size: cover;
    color: #093358;
  }
  .bg-night {
    background-image: url(../assets/night.jpeg);
    background-size: cover;
    
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
