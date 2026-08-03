<template>
  <Teleport to="body">
    <Transition name="modal-outer">
      <div
        v-show="modalActive"
        class="fixed inset-0 w-full h-screen bg-black/40 flex justify-center px-4 z-50"
        @click.self="closeModal"
      >
        <Transition name="modal-inner">
          <div
            v-if="modalActive"
            class="p-6 bg-white rounded-md self-start mt-24 max-w-screen-md w-full shadow-lg"
          >
            <slot />

            <button
              class="text-white mt-8 bg-weather-primary py-2 px-6 rounded-md hover:opacity-90 transition"
              @click="closeModal"
            >
              Close
            </button>
          </div>
        </Transition>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
const emit = defineEmits(["close-modal"]);

defineProps({
  modalActive: {
    type: Boolean,
    default: false,
  },
});

const closeModal = () => {
  emit("close-modal");
};
</script>

<style scoped>
.modal-outer-enter-active,
.modal-outer-leave-active {
  transition: opacity 0.25s ease;
}

.modal-outer-enter-from,
.modal-outer-leave-to {
  opacity: 0;
}

.modal-inner-enter-active {
  transition: all 0.25s ease 0.1s;
}

.modal-inner-leave-active {
  transition: all 0.2s ease;
}

.modal-inner-enter-from {
  opacity: 0;
  transform: scale(0.9);
}

.modal-inner-leave-to {
  opacity: 0;
  transform: scale(0.9);
}
</style>