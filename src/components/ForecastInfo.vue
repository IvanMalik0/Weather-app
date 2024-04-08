<script setup>
defineProps({
    city: {
        type: Object,
        required: true
    }
})
</script>

<template>
    <div class="absolute bg-white/80 backdrop-blur-sm text-slate-900 inset-x-0 bottom-0 rounded-t-lg p-8">
        <!-- Close button -->
        <div class="flex justify-end mb-10">
            <button class="p-1" @click="$emit('close-info')">
                <i class="fa-solid fa-xmark text-xl"></i>
            </button>
        </div>

        <div class="flex items-center justify-between gap-6 mb-20">

            <!-- Wind speed -->
            <div class="text-center flex-1">
                <i class="fa-solid fa-wind mb text-2xl"></i>
                <p class="text-xl font-bold">{{ city.current.wind_kph }} km/h</p>
                <p>Wind</p>
            </div>

            <!-- Humidity level -->
            <div class="text-center flex-1">
                <i class="fa-solid fa-droplet mb text-2xl"></i>
                <p class="text-xl font-bold">{{ city.current.humidity }}%</p>
                <p>Humidity</p>
            </div>

            <!-- Precipitation -->
            <div class="text-center flex-1">
                <i class="fa-solid fa-umbrella mb text-2xl"></i>
                <p class="text-xl font-bold">{{ city.current.precip_mm }} mm</p>
                <p>Precipitation</p>
            </div>
        </div>
        <div class="flex items-center justify-between gap-6 mb-10">

            <!-- Wind direction -->
            <div class="text-center flex-1">
                <i class="fa-solid fa-fan mb text-2xl"></i>
                <p class="text-xl font-bold">{{ city.current.wind_dir }}</p>
                <p>Direction</p>
            </div>

            <!-- Feels like -->
            <div class="text-center flex-1">
                <i class="fa-solid fa-temperature-half mb text-2xl"></i>
                <p class="text-xl font-bold">{{ Math.round(city.current.feelslike_c) }}</p>
                <p>Feels like</p>
            </div>

            <!-- UV -->
            <div class="text-center flex-1">
                <i class="fa-solid fa-sun mb text-2xl"></i>
                <p class="text-xl font-bold">{{ city.current.uv }}</p>
                <p>UV index</p>
            </div>
        </div>

        <!-- Last update and delete -->
        <div class="flex justify-between items-center">
            <h3 class="text-slate-900/50">last update: {{ city.current.last_updated }}</h3>
            <button @click="$emit('remove-city')">
                <i class="fa-solid fa-trash"></i>
            </button>
        </div>
    </div>
</template>