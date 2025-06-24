<script setup>
import { inject } from 'vue';
import AIHomePage from '../components/asianPaintsAI/AIHomePage.vue';

const rem = inject('remToPixel');
</script>

<template>
  <div class="home-page-wrapper">
    <AIHomePage />
  </div>
</template>

<style scoped lang="scss">
.home-page-wrapper {
  height: 100%;
  width: 100%;
  display: grid;
  overflow: hidden;
  color: $color-red;
  background-color: #fff;
}
</style>
