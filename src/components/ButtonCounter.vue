<template>
  <button @click="increment">Counter {{ counter }}</button>
</template>

<script setup>
import { ref } from "vue";

const counter = ref(0);
const increment = () => {
  counter.value++;
};
</script>

<style>
</style>