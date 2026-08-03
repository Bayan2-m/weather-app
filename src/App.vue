<template>
  <div class="flex flex-col min-h-screen font-Roboto bg-weather-primary">

    <!-- Navigation -->
    <SiteNavigation />

    <!-- Page Transition -->
    <RouterView v-slot="{ Component }">
      <Transition name="page" mode="out-in">
        <component :is="Component" />
      </Transition>
    </RouterView>

  </div>
</template>

<script setup>
import SiteNavigation from "./components/SiteNavigation.vue";
import { RouterView } from "vue-router";
</script>

<style scoped>
.page-enter-active,
.page-leave-active {
  transition: all 0.35s ease;
  will-change: transform, opacity;
}

.page-enter-from {
  opacity: 0;
  transform: translateY(12px);
}

.page-leave-to {
  opacity: 0;
  transform: translateY(-12px);
}
</style>