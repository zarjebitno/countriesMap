<template>
  <div>
    <Loader
      v-if="!currentCountry.country"
    />
    <div class="w-100 bg-sky-500 flex justify-center p-6" @click="test" v-if="currentCountry.country">
      {{ currentCountry.country[0]?.name.common }}
      {{ currentCountry.country[0]?.capital }}
      <img :src="currentCountry.country[0]?.flags.png" :alt="currentCountry.country[0]?.name.common + 'flag'"/>
      {{ currentCountry.country[0].name.nativeName.eng }}
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