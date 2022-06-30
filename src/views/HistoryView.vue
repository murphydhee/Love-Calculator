<script setup>
import History from "../components/History.vue";
</script>
<template>
  <div class="about">
    <h1>Search History Results</h1>
    <History/>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
