<script setup lang="ts">
defineProps<{
  msg: string
}>()
</script>

<template>
    <h1 class="green">{{ msg }}</h1>
    <div id="game"></div>
    <div id="playerOne"></div>
    <div id="playerTwo"></div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
  color: whitesmoke;
}
</style>
