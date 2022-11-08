<template>
  <Transition name="fade">
    <div>
      <h2> {{ props.country?.flag }} {{ props.country?.name.common }}</h2>
      <p>Population: {{ props.country?.population.toLocaleString('en-GB') }}</p>
      <nuxt-link :to="'/country/' + props.country?.name.common">Get more info</nuxt-link>
    </div>
  </Transition>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps(['country'])
</script>

<style>
  .fade-enter-active {
  animation: bounce-in 0.5s;
}
.fade-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.25);
  }
  100% {
    transform: scale(1);
  }
}
</style>