<script setup lang="ts">
import { ref } from "vue";
const count = ref(0);
</script>
<template>
  <button class="btn btn-secondary mx-2" @click="count++">
    You clicked me {{ count }} times.
  </button>
</template>
