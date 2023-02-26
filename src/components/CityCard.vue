<template lang="">
    <div class="flex justify-between py-6 px-3 bg-weather-secondary rounded-md shadow-md
    cursor-pointer">
        <div class="flex flex-col">
            <h2 class="text-3xl">{{ city.city }}</h2>
            <h3 class="">{{ city.state }}</h3>
        </div>
        <div class="flex flex-col">
            <div class="flex self-end">
                <!-- <img 
                class="flex items-end w-[70px] h-[50px] object-cover"
                :src="`http://openweathermap.org/img/wn/${city.weather.weather[0].icon}@2x.png`" alt="image"> -->
                <p class="text-3xl">
                    {{ Math.round(city.weather.main.temp) }}&deg;
                </p>
            </div>
            <div class="flex gap-2">
                <span class="text-s">
                    H: {{ Math.round(city.weather.main.temp) }}&deg;
                </span>
                <span class="text-s">
                    L: {{ Math.round(city.weather.main.temp_min) }}&deg;
                </span>
            </div>
        </div>
    </div>
</template>
<script setup>
defineProps({
    city: {
        type: Object,
        default: () => {},
    }
})
</script>
