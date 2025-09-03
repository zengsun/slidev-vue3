<template>
  <div class="form-control">
    <div class="relative">
      <input
        type="color"
        class="w-full pr-20 input input-primary input-bordered"
        v-model="color"
      />
      <button
        class="absolute top-0 right-0 m-0 rounded-l-none btn btn-primary"
        @click="$emit('pick', color)"
      >
        Pick
      </button>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
const color = ref("");
</script>
