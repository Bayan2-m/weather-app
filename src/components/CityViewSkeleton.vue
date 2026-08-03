<template>
  <div class="flex flex-col flex-1">

    <!-- 🌤️ Overview Skeleton -->
    <div class="flex flex-col py-12 items-center gap-4">

      <AnimatedPlaceholder class="max-w-[200px] w-full h-8" />

      <AnimatedPlaceholder class="max-w-[120px] w-full h-5" />

      <AnimatedPlaceholder class="max-w-[120px] w-full h-24 rounded-full" />

      <AnimatedPlaceholder class="max-w-[80px] w-full h-10" />

      <AnimatedPlaceholder class="max-w-[180px] w-full h-6" />

    </div>

    <hr class="w-full border-white/10" />

    <!-- ⏱️ Hourly Skeleton -->
    <div class="flex flex-col py-8 px-6">
      <AnimatedPlaceholder class="max-w-[200px] h-6 mb-6" />

      <div class="flex gap-4 overflow-hidden">
        <AnimatedPlaceholder class="w-20 h-24" />
        <AnimatedPlaceholder class="w-20 h-24" />
        <AnimatedPlaceholder class="w-20 h-24" />
        <AnimatedPlaceholder class="w-20 h-24" />
        <AnimatedPlaceholder class="w-20 h-24" />
      </div>
    </div>

    <hr class="w-full border-white/10" />

    <!-- 📅 Weekly Skeleton -->
    <div class="flex flex-col py-8 px-6">

      <AnimatedPlaceholder class="max-w-[200px] h-6 mb-6" />

      <div class="flex flex-col gap-3">

        <AnimatedPlaceholder class="w-full h-10" />
        <AnimatedPlaceholder class="w-full h-10" />
        <AnimatedPlaceholder class="w-full h-10" />
        <AnimatedPlaceholder class="w-full h-10" />
        <AnimatedPlaceholder class="w-full h-10" />
        <AnimatedPlaceholder class="w-full h-10" />
        <AnimatedPlaceholder class="w-full h-10" />

      </div>
    </div>

  </div>
</template>

<script setup>
import AnimatedPlaceholder from "./AnimatedPlaceholder.vue";
</script>