<script setup>
import { ref, watch } from 'vue';
import { useToast } from '../lib/stores.js';

const { toast } = useToast();
const isVisible = ref(false);

watch(() => toast.id, () => {
  if (toast.message) {
    isVisible.value = true;
    setTimeout(() => {
      isVisible.value = false;
    }, 3000);
  }
});
</script>

<template>
  <Transition name="toast">
    <div
      v-if="isVisible"
      class="fixed top-5 left-1/2 -translate-x-1/2 z-[100] px-5 py-3 rounded-lg shadow-lg text-white font-semibold text-sm"
      :class="{
        'bg-green-500': toast.type === 'success',
        'bg-red-500': toast.type === 'error'
      }"
    >
      {{ toast.message }}
    </div>
  </Transition>
</template>

<style scoped>
.toast-enter-active,
.toast-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}
.toast-enter-from,
.toast-leave-to {
  opacity: 0;
  transform: translateY(-20px) translateX(-50%);
}
.toast-enter-to,
.toast-leave-from {
    transform: translateY(0) translateX(-50%);
}
</style>