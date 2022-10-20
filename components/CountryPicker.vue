<template>
  <div id="country-picker">
    <label for="countries" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-400">Select an option</label>
    <select id="countries" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" @change="handleChange">
      <option selected>Choose a country</option>
      <option v-for="(country, i) in props.countryNames" :key="i" :value="country.cca2">{{ country.name.common }}</option>
      
    </select>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, computed } from 'vue'

const props = defineProps(['countryNames'])

const emit = defineEmits(['changeValue'])

// const orderedCountries = computed(() => props.countryNames.sort((a, b) => (a.name.common > b.name.common) ? 1 : -1))

const handleChange = () => {
  const selectValue = document.querySelector('#countries').value
  emit('changeValue', selectValue)
}

</script>