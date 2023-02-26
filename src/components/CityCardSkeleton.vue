<template lang="">
    <div class="flex justify-between py-6 px-3 bg-weather-secondary rounded-md shadow-md
    cursor-pointer">
        <div class="flex flex-col flex-1 gap-2">
            <AnimatedPlaceholder class="max-w-[50%]"/>
            <AnimatedPlaceholder class="max-w-[40%] mb-auto"/>
        </div>
        
        <div class="flex flex-col gap-2 items-end flex-1">
            <AnimatedPlaceholder class="max-w-[50px] w-full"/>
            <AnimatedPlaceholder class="max-w-[75px] w-full"/>
        </div>
    </div>
</template>

<script setup>

import AnimatedPlaceholder from './AnimatedPlaceholder.vue';

</script>
