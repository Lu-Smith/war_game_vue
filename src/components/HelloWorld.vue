<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';
defineProps<{
  msg: string
}>();

let gameOver = ref(true), 
    cardOne = ref({}), 
    cardTwo = ref({}), 
    deckId = ref(null),
    playerOneScore = ref(0),
    playerTwoScore = ref(0)

async function getDeck() {
  gameOver.value = false;
  if(deckId.value == null) {
    //create a new deck
    const { data } = await axios.get(
      "https://www.deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1"
    );
    deckId.value = data.deckId
    console.log(data)
  };
}
async function getCards() {
  const { data } = await axios.get(
      "https://www.deckofcardsapi.com/api/deck/" + deckId.value + "/draw/?count=2"
  );
}

getDeck();
getCards();
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
