<script setup>
import { ref } from 'vue'
import SearchInput from '@/components/SearchInput.vue'
import CurrentForecastCard from '@/components/CurrentForecastCard.vue'


const cities = ref([])

const addCity = (data) => {
  cities.value.push(data)
}

const deleteCity = (name) => {
  if (confirm('Are you sure?')) {
    cities.value = cities.value.filter((p) => p.location.name !== name)
  }
}
</script>

<template>
  <main>
    <!-- Date -->
    <div class="text-center mb-6">
      {{ new Date().toLocaleDateString('en-us', {
        weekday: 'long',
        year: 'numeric',
        month: 'long',
        day: 'numeric'
      }) }}
    </div>

    <!-- Search -->
    <div>
      <SearchInput @city-data="addCity" />
    </div>

    <!-- Forecast cards -->
    <div class="grid grid-cols-3 gap-3">
      <div v-for="(city, index) in cities" :key="index">
        <CurrentForecastCard :city="city" @delete-city="deleteCity" />
      </div>
    </div>
  </main>
</template>