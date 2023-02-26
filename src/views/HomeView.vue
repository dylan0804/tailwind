<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input 
      type="text" 
      @input="getSearchResults"
      placeholder="Search for a city or state"
      v-model="searchQuery"
      class="py-2 px-1 w-full bg-transparent border-b 
      focus:border-weather-secondary focus:outline-none
      focus:shadow-[0px_1px_0_0#004E71]">

      <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]" 
      v-if="mapboxSearchResults">
      <p class="p-2" v-if="searchError">Sorry, something went wrong</p>
      <p class="p-2" v-if="!searchError && mapboxSearchResults.length === 0">No results match your query</p>
        <template v-else>
          <li v-for="searchResult in mapboxSearchResults"
          :key="searchResult.id"
          @click="previewCity(searchResult)"
          class="p-2 cursor-pointer">
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-4">
      <Suspense>
        <CityList />
        <template #fallback>
          <CityCardSkeleton />
        </template>
      </Suspense>
    </div>
  </main>
</template>

<script>
import axios from "axios"
import { useRouter } from "vue-router"
import CityList from "@/components/CityList.vue"
import CityCardSkeleton from "@/components/CityCardSkeleton.vue"

export default {
  name: 'HomeView',
  components: {
    CityCardSkeleton,
    CityList,
  },
  data() {
    return {
      searchQuery: '',
      queryTimeout: null,
      mapboxAPIKey: 'pk.eyJ1IjoiZHlsYW4wODA0IiwiYSI6ImNsZWk4MHptbTFodDkzdW1yZXlmeTd1NGYifQ.EQ1QtowxY4Ye4G8yfOv9Bg',
      mapboxSearchResults: null,
      searchError: null,
      router: useRouter(),
    }
  },
  methods: {
    getSearchResults() {
      clearTimeout(this.queryTimeout)
      this.queryTimeout = setTimeout(async () => {
        if(this.searchQuery !== '') {
          try {
            const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${this.searchQuery}.json?access_token=${this.mapboxAPIKey}&types=place`)
            this.mapboxSearchResults = result.data.features;
          } catch {
            this.searchError = true
          }
          return;
        }
        this.mapboxSearchResults = null
      }, 300)
    },

    previewCity(searchResult) {
      console.log(searchResult)
      const [city, state] = searchResult.place_name.split(",")
      this.router.push({
        name: 'CityView',
        params: {state: state.replaceAll(" ", ""), city: city},
        query: {
          lat: searchResult.geometry.coordinates[1],
          lng: searchResult.geometry.coordinates[0],
          preview: true
        }
      })
    }
  },  
  
}
</script>
