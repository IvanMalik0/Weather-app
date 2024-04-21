<script setup>
import { reactive } from 'vue'
import { defineEmits } from 'vue'

const emit = defineEmits('city-data')

const searchTerm = reactive({
    query: '',
    timeout: null,
    results: '',
})

const handleSearch = () => {
    clearTimeout(searchTerm.timeout)
    searchTerm.timeout = setTimeout(async () => {
        if (searchTerm.query !== '') {
            const res = await fetch(`http://api.weatherapi.com/v1/search.json?key=5cf6c1aac2d54e0eb8c171146242104&q=${searchTerm.query}`)

            const data = await res.json()
            searchTerm.results = data
        } else {
            searchTerm.results = ''
        }
    }, 500)
}

const getForecast = async (id) => {
    const res = await fetch(`http://api.weatherapi.com/v1/forecast.json?key=5cf6c1aac2d54e0eb8c171146242104&q=id:${id}&days=3&aqi=no&alerts=no
`)
    const data = await res.json()
    emit('city-data', data)

    searchTerm.query = ''
    searchTerm.results = ''
}
</script>

<template>
    <div>
        <!-- Search field -->
        <form @submit.prevent="onSubmit">
            <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center">
                <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
                <input type="text" placeholder="Search for a place"
                    class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
                    v-model="searchTerm.query" @input="handleSearch" />
            </div>
        </form>
        <!-- Search suggestions -->
        <div class="bg-white my-2 rounded-lg shadow-lg">
            <div v-if="searchTerm.results !== ''">
                <div v-for="city in searchTerm.results" :key="city.id">
                    <button class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left"
                        @click="getForecast(city.id)">
                        {{ city.name }}, {{ city.region }}, {{ city.country }}
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>