<template>
  <transition>
      <div id="country-picker" class="flex flex-col items-center">
        <label for="countries" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-400">Select an option</label>
        <select id="countries" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 w-2/4 mb-4" @change="handleChange">
          <option selected>Choose a country</option>
          <option v-for="(country, i) in orderedCountries" :key="i" :value="country.cca2">
            {{ country.flag + " " + country.name.common }}
          </option>
        </select>
      </div>
  </transition>
</template>

<script setup>
import { defineProps, defineEmits, computed, watch } from 'vue'

const props = defineProps(['countryNames', 'activeCountry'])

const emit = defineEmits(['changeValue'])

const orderedCountries = computed(() => {
  let toBeOrderedCountries = [...props.countryNames]
   return toBeOrderedCountries.sort((a, b) => {
      const x = a.name.common
      const y = b.name.common

      if(x > y) return 1
      if(y > x) return -1
      return 0
    })
})

const handleChange = () => {
  const selectValue = document.querySelector('#countries').value
  emit('changeValue', selectValue)
}

watch(props, newValue => {
    const selectedCountryCode = newValue.activeCountry
    const selectList = document.querySelector('#countries')

    selectList.value = selectedCountryCode
  })

</script>