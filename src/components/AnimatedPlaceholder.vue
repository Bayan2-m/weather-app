<template>
  <div
    class="relative overflow-hidden rounded bg-white/20"
    :style="{ width, height }"
  >
    <!-- shimmer layer -->
    <div class="shimmer"></div>
  </div>
</template>

<script setup>
defineProps({
  width: {
    type: String,
    default: "100%",
  },
  height: {
    type: String,
    default: "1.5rem",
  },
});
</script>

<style scoped>
/* shimmer effect */
.shimmer {
  position: absolute;
  top: 0;
  left: -150%;
  height: 100%;
  width: 150%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.25),
    transparent
  );
  animation: shimmer 1.2s infinite;
}

@keyframes shimmer {
  100% {
    left: 100%;
  }
}
</style>