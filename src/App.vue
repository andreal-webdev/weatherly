<script setup>
import { ref } from 'vue';
import searchBar from './components/searchBar.vue'; // Adjusted component names
import cardWeather from './components/cardWeather.vue'; // Adjusted component names

import userLocation from './components/userLocation.vue'; // Adjusted component name

const places = ref([])
const addPlace = (data) => {
  places.value.push(data)
}

const deleteCity = (name) => {
  if (confirm('Are you sure?')) {
    places.value = places.value.filter((p) => p.location.name !== name)
  }
}
</script>

<template>
  <main>

    <!-- Navigation Bar -->
    <nav class="bg-[#093358] flex items-center justify-between text-white text-[16px] py-2 px-3 mb-20">
      <a class="" href="index.html"><img src="./assets/logo-blue-bg.png" alt="" width="60"></a>
      <!-- This displays the current date -->
      <div class="hidden md:block text-center text-sm md:text-base">
        <i class="fa-regular fa-calendar text-[#24ceff]"></i>
        {{
          new Date().toLocaleDateString('en-us', {
            weekday: 'long',
            year: 'numeric',
            month: 'long',
            day: 'numeric'
          })
        }}
      </div>
      <!-- This displays the user's browser location -->
      <div class="text-white text-sm md:text-base">   
        <userLocation />
      </div>
    </nav>
    <!-- Header Content -->
    <div class="container-fluid mb-20" id="hero">
      <div class="grid grid-cols-1 md:grid-cols-2 p-5">
        <div class="flex flex-col justify-center md:ml-5">
          <div class="hero-text text-white pb-3">
            <h1 class="text-4xl md:text-5xl mb-3 hero-title font-bold">Welcome to Weatherly<span class="text-[#F2B705] text-4xl">!</span></h1>
            <p class="text-lg md:text-xl mb-2">Get accurate weather forecasts for the next 3 days and plan your activities with confidence.</p>
            <p class="text-[#F2B705] text-lg md:text-xl">Add as many locations as you want <i class="fa-solid fa-location-crosshairs"></i></p>
          </div>
          
          <!-- Search Bar -->
          <div class="w-full md:w-auto md:max-w-md">   
            <searchBar @place-data="addPlace" class="p-3 md:p-0 text-sm md:text-base"></searchBar>
          </div>
        </div>

        <div class="hidden md:flex items-center justify-center">
          <img class="img-responsive" src="./assets/hero.png" width="300">
        </div>
      </div>
    </div>


    <!--Card-->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-4">
      <div v-for="(place, idx) in places" :key="idx" class="w-full md:auto">
        <cardWeather :place="place" @delete-city="deleteCity" />
      </div>
    </div>


  </main>
</template>

