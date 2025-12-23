<script setup lang="ts">
import Calculator from '../components/SubnetCalculator.vue';
</script>

<template>
  <div class="container">
    <Calculator />
  </div>
</template>

<style scoped>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
}
</style>
