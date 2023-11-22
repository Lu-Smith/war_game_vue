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
    deckId.value = data.deck_id
  };
};

async function getCards() {
  const { data } = await axios.get(
      "https://www.deckofcardsapi.com/api/deck/" + deckId.value + "/draw/?count=2"
  );

  const remaining = data.cards.remaining;
  const { cards } = data;
  cardOne.value = cards[0];
  cardTwo.value = cards[1];
  console.log(cardOne.value.images.png)
};

getDeck();

</script>

<template>
    <h1 class="green">{{ msg }}</h1>
    <div id="game"></div>
    <div id="playerOne">
      <h4>Player One</h4>
      <div class="card">
        <img :src="cardOne?.images?.png" alt="">
      </div>
    </div>
    <div id="playerTwo">
      <h4>Player Two</h4>
      <div class="card">
        <img :src="cardTwo?.images?.png" alt="">
      </div>
    </div>
    <button @click="getCards()">DrawCards</button>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
  color: whitesmoke;
}

h4 {
  color: whitesmoke;
}

.card {
  color: whitesmoke;
}
</style>
