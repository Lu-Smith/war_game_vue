<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';
defineProps<{
  msg: string
}>();

let gameOver = ref(true), 
    cardOne = ref({}), 
    cardTwo = ref({}), 
    checkId = ref(null),
    playerOneScore = ref(0),
    playerTwoScore = ref(0)

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
