<script setup lang="ts">
import { ref } from "vue";
// import { } from '@vueuse/core'
const isActive = ref(true),
  hasError = ref(false);
</script>
<template>
  <div class="mx-2">
    <div class="text-center m-4 p-4" :class="{ active: isActive, error: hasError }">
      Active Class
    </div>
    <div class="flex gap-4">
      <button class="btn" @click="isActive = !isActive">
        isActive
      </button>
      <button class="btn" @click="hasError = !hasError">
        hasError
      </button>
    </div>

  </div>
</template>
<style>
.active {
  color: rebeccapurple;
}

.error {
  background: antiquewhite; 
}
</style>
