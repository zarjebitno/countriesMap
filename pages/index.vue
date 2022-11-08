<template>
  <div v-if="listOfCountries">
    <ErrorInfo v-if="isError" :error-msg="errorMsg"/>
    <div class="map-container w-10/12 mx-auto py-8">
      <h1 class="text-6xl text-center mb-2">Pick a country!</h1>
      <p class="text-center text-sm mb-10">
        {{ activeCountryComp }}
      </p>
      <UIToggleSwitch
        :isOpen="isCountryPickerOpen"
        @isToggled="selectListViewToggled"
      />
      <CountryPicker
        v-show="isCountryPickerOpen"
        :countryNames="listOfCountries"
        :activeCountry="activeCountry?.cca2"
        @changeValue="handleChange"
      />
      <div class='flex'>
        <Map
          @onCountryClick="handleClick"
          :class="{active : activeCountry}"
          :activeCountry="activeCountry?.cca2"
        />
        <CountryInfo
          :country="activeCountry"
          v-if="activeCountry"
        />
      </div>
    </div>
  </div>
  <Loader v-else/>
</template>

<script setup>
import { onBeforeMount, ref, computed } from "vue"

  const listOfCountries = ref(null)
  const activeCountry = ref(null)
  const isCountryPickerOpen = ref(false)
  let isError = ref(false)
  let errorMsg = ref(null)

  // set active country
  const handleClick = (countryId) => {
    // try catch for catching calls to not loaded data
    let country;

    try {
      country = listOfCountries.value.find((country) => countryId === country.cca2)
    } catch(e) {
      console.warn(e)
    }

    if(country != undefined && country != null) {
      activeCountry.value = country
      isError.value = false
    }
    else {
      activeCountry.value = null
      isError.value = true
      errorMsg.value = "No Data."

      setTimeout(() => {
        isError.value = false
      }, 2500)
    }
  }

  const handleChange = (selectInputValue) => {
    activeCountry.value = listOfCountries.value.find(country => country.cca2 == selectInputValue)
  }

  const selectListViewToggled = () => {
    isCountryPickerOpen.value = !isCountryPickerOpen.value
  }

  // computed props
  const activeCountryComp = computed(() => {
    return activeCountry.value ? activeCountry.value.flag + " " + activeCountry.value.name?.common : "No country selected"
  })

  // populate list of countries
  // top level await not permitted so lifecycle hook it is
  onBeforeMount(async () => {
    const response = await fetch('https://restcountries.com/v3.1/all')
    listOfCountries.value = await response.json()
  })

</script>
