<script setup>
import { ref } from 'vue';
import images from "./../data/images";

/**
 * initial values
 * @isActive responsible for active item
 * @interval responsible for delay auto slide between two images
 */
let isActive = ref(0)
const interval = 5000 //milliseconds

// navigation to Previous image
const prev = () => {
  if (isActive.value === 0) {
    isActive.value = (images.length - 1)
  } else {
    isActive.value -= 1
  }
}

// navigation to Next image
const next = () => {
  if (isActive.value === (images.length - 1)) {
    isActive.value = 0
  } else {
    isActive.value += 1
  }
}

// autoplay  
let initAutoPlay = setInterval(() => {
  next()
}, interval)

// to active autoplay on page load
window.onload = () => {
  initAutoPlay
}

// on mouse out autoplay will be restart
const replay = () => {
  initAutoPlay = setInterval(() => {
    next()
  }, interval)
}

// on mouse over autoplay will be stopped
const hold = () => {
  clearInterval(initAutoPlay)
}

</script>
<template>
  <section class="max-w-2xl mx-auto mt-8">
    <div @mouseover="hold" @mouseout="replay" class="relative w-full">
      <!-- Carousel wrapper -->
      <div class="relative h-[470px] overflow-hidden rounded-2xl bg-slate-200">
        <template v-for="(image, index) in images" :key="index">
          <Transition name="slide-fade">
            <div class="duration-700 ease-in-out" v-show="isActive === index">
              <img :src="image.thumb" class="absolute block w-full -translate-x-1/2 -translate-y-1/2 top-1/2 left-1/2"
                alt="...">
            </div>
          </Transition>
        </template>
      </div>
      <!-- Slider indicators -->
      <div
        class="absolute z-30 flex px-3 py-2 space-x-3 -translate-x-1/2 border rounded-full shadow-md bg-white/50 bottom-5 left-1/2">
        <button v-for="(image, index) in images" :key="index" @click="isActive = index" type="button"
          class="w-3 h-3 rounded-full" :class="isActive === index ? 'bg-green-400' : 'bg-black'"></button>
      </div>
      <!-- Slider controls -->
      <button @click="prev" type="button"
        class="absolute top-0 left-0 z-30 flex items-center justify-center h-full px-4 cursor-pointer group focus:outline-none">

        <span
          class="inline-flex items-center justify-center w-10 h-10 rounded-full bg-white/30 dark:bg-gray-800/30 group-hover:bg-white/50 dark:group-hover:bg-gray-800/60 group-focus:ring-4 group-focus:ring-white dark:group-focus:ring-gray-800/70 group-focus:outline-none">
          <svg class="w-4 h-4 text-white dark:text-gray-800" xmlns="http://www.w3.org/2000/svg" fill="none"
            viewBox="0 0 6 10">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="M5 1 1 5l4 4" />
          </svg>
          <span class="sr-only">Previous</span>
        </span>

      </button>

      <button @click="next" type="button"
        class="absolute top-0 right-0 z-30 flex items-center justify-center h-full px-4 cursor-pointer group focus:outline-none">

        <span
          class="inline-flex items-center justify-center w-10 h-10 rounded-full bg-white/30 dark:bg-gray-800/30 group-hover:bg-white/50 dark:group-hover:bg-gray-800/60 group-focus:ring-4 group-focus:ring-white dark:group-focus:ring-gray-800/70 group-focus:outline-none">
          <svg class="w-4 h-4 text-white dark:text-gray-800" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
            fill="none" viewBox="0 0 6 10">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="m1 9 4-4-4-4" />
          </svg>
          <span class="sr-only">Next</span>
        </span>

      </button>

    </div>
  </section>
</template>
<style scoped>
.slide-fade-enter-active {
  transition: all 2s ease-in-out;
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  /* transform: translateY(5px); */
  opacity: 0.1;
}
</style>
