<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import Info from '@/components/Info.vue'
</script>

<template>
  
  <Info />
  <div class="container">
    <div class="center">
      <RouterView />
    </div>
  </div>

</template>

<style scoped>


.dev {
  position: sticky;
  color: red;
  font-weight: bold;

}

.center {
  margin: 0 auto;
  top: 25vh;
}

</style>
