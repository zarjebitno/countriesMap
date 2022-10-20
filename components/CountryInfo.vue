<template>
  <div>
    <h2> {{ props.country?.flag }} {{ props.country?.name.common }}</h2>
    <p>Population: {{ props.country?.population.toLocaleString('en-GB') }}</p>
    <nuxt-link :to="'/country/' + props.country?.name.common">Get more info</nuxt-link>
  </div>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps(['country'])
</script>