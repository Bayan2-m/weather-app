<template>
  <div
    class="flex py-6 px-3 bg-primary-green rounded-md shadow-md animate-pulse"
  >
    <!-- Left: City info -->
    <div class="flex flex-col flex-1 gap-2">
      <AnimatedPlaceholder class="max-w-[60%] h-6" />
      <AnimatedPlaceholder class="max-w-[40%] h-4" />
    </div>

    <!-- Right: Weather -->
    <div class="flex flex-col items-end flex-1 gap-2">
      <AnimatedPlaceholder class="max-w-[60px] w-full h-8" />
      <AnimatedPlaceholder class="max-w-[90px] w-full h-4" />
    </div>
  </div>
</template>

<script setup>
import AnimatedPlaceholder from "./AnimatedPlaceholder.vue";
</script>