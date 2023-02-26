<template lang="">
    <header class="sticky top-0 bg-weather-primary shadow-lg">
        <nav class="container flex flex-col gap-4 py-6 items-center sm:flex-row text-white">
            <RouterLink :to="{name: 'home'}">
                <div class="flex items-center gap-3 ">
                    <i class="fa-solid fa-sun text-2xl"></i>
                    <p class="text-2xl">The Local Weather</p>
                </div>
            </RouterLink>
            <div class="flex gap-3 flex-1 justify-end">
                <i @click="toggleModal" class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 curs"></i>
                <i v-if="this.route.query.preview" @click="addCity" class="fa-solid fa-plus text-xl  hover:text-weather-secondary duration-150 curs"></i>
            </div>
            <BaseModal :isModalOn="isModalOn" @toggleModal=toggleModal> 
                <div class="text-black">
                    <h1 class="text-2xl mb-1">About:</h1>
                    <p class="mb-4">
                        The Local Weather allows you to track the current and
                        future weather of cities of your choosing.
                    </p>
                    <h2 class="text-2xl">How it works:</h2>
                    <ol class="list-decimal list-inside mb-4">
                        <li>
                        Search for your city by entering the name into the
                        search bar.
                        </li>
                        <li>
                        Select a city within the results, this will take
                        you to the current weather for your selection.
                        </li>
                        <li>
                        Track the city by clicking on the "+" icon in the
                        top right. This will save the city to view at a
                        later time on the home page.
                        </li>
                    </ol>
                    <h2 class="text-2xl">Removing a city</h2>
                    <p>
                        If you no longer wish to track a city, simply select
                        the city within the home page. At the bottom of the
                        page, there will be am option to delete the city.
                    </p>
                </div>
            </BaseModal>
        </nav>
    </header>
</template>
<script>
import { RouterLink, useRoute, useRouter } from 'vue-router';
import BaseModal from './BaseModal.vue';
import { uid } from 'uid';

export default {
    name: 'SiteNavigation',
    components: {
        BaseModal,
    },
    data() {
        return {
            isModalOn: false,
            savedCities: [],
            route: useRoute(),
            router: useRouter(),
        }
    },
    methods: {
        toggleModal() {
            this.isModalOn = !this.isModalOn
        },
        addCity() {
            if(localStorage.getItem('savedCities')) {
                this.savedCities = JSON.parse(localStorage.getItem('savedCities'))
                console.log(this.savedCities)
            }
            const locationObj = {
                id: uid(),
                state: this.route.params.state,
                city: this.route.params.city,
                coords: {
                    lat: this.route.query.lat,
                    lng: this.route.query.lng,
                }
            }

            this.savedCities.push(locationObj)
            localStorage.setItem('savedCities', JSON.stringify(this.savedCities))

            let query = Object.assign({}, this.route.query)
            delete query.preview;
            query.id = locationObj.id
            this.router.replace({query})
        }
    }
}
</script>
