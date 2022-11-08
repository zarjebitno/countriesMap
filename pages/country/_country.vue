<template>
  <div>
    <Loader
      v-if="!currentCountry.country"
    />
    <div class="country-main relative" v-else>
      <div class="img-wrapper h-96 w-full">
        <!-- TODO: find an api with random landmark image based on country good luck lol -->
        <img src="@/public/assets/images/sample_image.jpeg" :alt="currentCountry.country[0]?.name.common" class="w-full h-full object-cover brightness-75">
      </div>
      <div class="country-hero text-center absolute left-2/4 top-24 drop-shadow-lg">
        <h1 class="text-6xl text-white mb-4 font-extrabold tracking-widest">{{ currentCountry.country[0]?.name.common }}</h1>
        <p class="text-xl text-white uppercase font-extralight">Random Tourist Motto</p>
      </div>
      <div class="country-floatee absolute left-2/4 top-80 px-16 py-5 flex justify-between items-center w-2/4 rounded-3xl shadow-xl shadow-slate-400">
        <!-- refactor to ui components -->
        <div class="card flex flex-col items-center">
          <div class="card-image-wrapper">
            <img src="@/public/assets/images/icons/buildings.svg" alt="" class="w-16 h-auto"/>
          </div>
          <span class="mt-2" v-for="(capitals, i) in currentCountry.country[0]?.capital" :key="i">{{capitals}}</span>
        </div>
        <div class="card flex flex-col items-center">
          <img src="https://img.icons8.com/external-becris-lineal-becris/512/external-population-environment-becris-lineal-becris.png" alt="" class="w-16 h-auto"/>
          <span class="mt-2">{{currentCountry.country[0]?.population.toLocaleString('en-GB')}}</span>
        </div>
        <div class="card">
          <img :src="currentCountry.country[0]?.coatOfArms.png" alt="Coat of Arms" class="w-24 h-auto"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// bypass invalid component name error idk
export default {
  name: "country"
}
</script>

<script setup>
import { useRoute } from '@nuxtjs/composition-api'
import { reactive, onBeforeMount } from 'vue'

const router = useRoute()
const countryName = router.value.params.country
let currentCountry = reactive({
  country: null
})

// populate list of countries
onBeforeMount(async () => {
  try {
    const response = await fetch('https://restcountries.com/v3.1/name/' + countryName)
    currentCountry.country = await response.json()
  } catch(ex) {
    console.log(ex)
  }
})

</script>

<style scoped>
  .country-hero {
    transform: translate(-50%)
  }

  .country-main .img-wrapper {
    background-color: rgba(0, 0, 0, .03);
  }

  .country-floatee {
    transform: translate(-50%, 0);
    background-color: rgba(203, 213, 225, .8);
  }
</style>