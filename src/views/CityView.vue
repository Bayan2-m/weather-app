<template>
  <div>
    <Suspense>
      <template #default>
        <AsyncCityView />
      </template>

      <template #fallback>
        <CityViewSkeleton />
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import { defineAsyncComponent } from "vue";
import CityViewSkeleton from "../components/CityViewSkeleton.vue";

/**
 * 🚀 أفضل ممارسة: تحميل Async component بشكل lazy
 */
const AsyncCityView = defineAsyncComponent(
  () => import("../components/AsyncCityView.vue")
);
</script>