<template>
  <Transition name="slide-fade">
    <div id="error-info" class="absolute top-12 right-8 px-4 py-1 text-white opacity-0">
      <p>{{errorMsg}}</p>
    </div>
  </Transition>
</template>

<script setup>
const props = defineProps({
  errorMsg: String
})
</script>

<style scoped>
  #error-info {
    background: #f2b233;
  }

.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}

</style>