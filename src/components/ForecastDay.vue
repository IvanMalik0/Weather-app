<script setup>
defineProps({
    day: {
        type: Object,
        required: true
    }
})
</script>

<template>
    <div class="mb-2 flex items-center justify-between">
        <table class="w-full">
            <tr>
                <!-- Day of the week -->
                <td class="w-1/3">{{ new Date(day.date).toLocaleDateString('en-us', { weekday: 'long' }) }}</td>

                <!-- Icon -->
                <td class="w-1/3">
                    <img :src="day.day.condition.icon" alt="icon" width="30" class="mx-auto" />
                </td>

                <!-- High/Low temp -->
                <td class="w-1/3 text-right">{{ Math.round(day.day.maxtemp_c) }} &deg; / {{ Math.round(day.day.mintemp_c) }}&deg;
                </td>
            </tr>
        </table>
    </div>
</template>