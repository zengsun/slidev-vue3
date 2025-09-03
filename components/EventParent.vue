<template>
  <div class="flex flex-row items-center">
    <h1>Colors:</h1>
    <div
      class="h-8 w-8 m-1"
      :style="{ background: color }"
      v-for="color in colors"
    />
    <event-child class="w-40" @pick="colors.push($event)" />
  </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
const colors = ref(["red", "green", "blue"]);
</script>
