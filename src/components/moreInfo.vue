<script setup>
defineProps({
    place: Object
})

</script>

<template>
  <div
    class="absolute bg-[#ffffff80] backdrop-blur-sm text-[#093358] inset-x-0 bottom-0 rounded-t-lg p-2 sm:p-1">
    <!-- Close button -->
    <div class="flex justify-end mb-10">
      <button @click="$emit('close-info')" class="p-1">
        <i class="fa-solid fa-xmark text-xl"></i>
      </button>
    </div>

    <div class="flex items-center justify-between gap-6 mb-10">
      <!-- Wind speed -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-wind mb text-2xl"></i>
        <p class="text-lg md:text-xl font-bold">{{ place.current.wind_kph }} km/h</p>
        <p>wind</p>
      </div>
      <!-- Humidity level -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-droplet mb text-2xl"></i>
        <p class="text-lg md:text-xl font-bold">{{place.current.humidity}} %</p>
        <p>humidity</p>
      </div>
      <!-- Precipitation -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-umbrella mb text-2xl"></i>
        <p class="text-lg md:text-xl font-bold">{{place.current.precip_mm}} mm</p>
        <p>precipitation</p>
      </div>
    </div>
    <div class="flex items-center justify-between gap-6 mb-10">
      <!-- Wind direction -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-fan mb text-2xl"></i>
        <p class="text-lg md:text-xl font-bold">{{ place.current.wind_dir }}</p>
        <p>direction</p>
      </div>
      <!-- Feels like -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-temperature-half mb text-2xl"></i>
        <p class="text-lg md:text-xl font-bold">{{Math.round(place.current.feelslike_c)}}&deg;</p>
        <p>Feels</p>
      </div>
      <!-- UV -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-sun mb text-2xl"></i>
        <p class="text-lg md:text-xl font-bold">{{place.current.uv}}</p>
        <p>UV index</p>
      </div>
    </div>
    
  </div>
</template>
