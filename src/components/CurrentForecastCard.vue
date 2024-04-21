<script setup>
import { ref } from 'vue'
import { defineEmits } from 'vue'
import BorderLine from './BorderLine.vue'
import ForecastDay from './ForecastDay.vue'
import ForecastInfo from './ForecastInfo.vue'

defineProps({
    city: {
        type: Object,
        required: true
    }
})
const showInfo = ref(false)

const emit = defineEmits(['delete-city'])

const removeCity = (cityName) => {
    emit('delete-city', cityName)
    showInfo.value = false
}
</script>

<template>
    <div :class="city.current.is_day === 1 ? 'bg-day' : 'bg-night'"
        class="text-white p-10 rounded-lg shadow-lg gap-6 mb-6 relative overflow-hidden">
        <!-- Location & time -->
        <div class="mb-2 flex justify-between items-center">
            <div class="flex items-center justify-center gap-2">
                <i class="fa-solid fa-location-dot"></i>
                <h1 class="text-3xl">{{ city.location.name }}</h1>
            </div>
            <div class="flex items-center justify-center gap-2">
                <i class="fa-solid fa-clock"></i>
                <h1 class="text-3xl">{{ new Date(city.location.localtime).getHours() }}:{{ new
        Date(city.location.localtime).getMinutes().toString().padStart(2, '0') }} </h1>
            </div>
        </div>

        <!-- Current weather -->
        <div class="text-center flex-1">
            <img :src="city.current.condition.icon" alt="icon" width="200" class="mx-auto -mb-10" />
            <h1 class="text-9xl mb-2 -mr-12">{{ Math.round(city.current.temp_c) }}&deg;</h1>
            <p class="text-2xl">{{ city.current.condition.text }}</p>
        </div>

        <BorderLine />

        <!-- Forecast -->
        <div v-for="(day, index) in city.forecast.forecastday" :key="index">
            <ForecastDay :day="day" />
        </div>

        <!-- Info -->
        <transition name="fade">
            <div v-show="showInfo">
                <ForecastInfo :city="city" @close-info="showInfo = false"
                    @remove-city="removeCity(city.location.name)" />
            </div>
        </transition>

        <!-- Forecast btn -->
        <div class="flex justify-end items-center gap-1 mt-10">
            <button @click="showInfo = true">More <i class="fa-solid fa-arrow-right text-sm -mb-px"></i></button>
        </div>
    </div>
</template>

<style scoped>
.bg-day {
    background: rgb(0, 212, 194);
    background: linear-gradient(153deg, rgba(0, 212, 194, 1) 0%, rgba(209, 212, 0, 1) 100%);
}

.bg-night {
    background: rgb(0, 2, 212);
    background: linear-gradient(153deg, rgba(0, 2, 212, 1) 0%, rgba(105, 0, 212, 1) 100%);
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>