<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';
defineProps<{
  msg: string
}>();

function translateCards(value) {
  switch (value) {
    case "JACK":
      value = "11";
      break;
    
    case "QUEEN":
      value = "12";
      break;

    case "KING":
      value = "13";
      break;

    case "ACE":
      value = "14";
      break;

    default:
    break;
  }
  return value;
}

let gameOver = ref(true), 
    cardOne = ref({}), 
    cardTwo = ref({}), 
    deckId = ref(null),
    playerOneScore = ref(0),
    playerTwoScore = ref(0);

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

  const remaining = data.remaining;
  const { cards } = data;
  cardOne.value = cards[0];
  cardTwo.value = cards[1];
  const valueOne = parseInt(translateCards(cardOne.value.value));
  const valueTwo = parseInt(translateCards(cardTwo.value.value));
  if(valueOne > valueTwo) playerOneScore.value += 1;
  if(valueOne < valueTwo) playerTwoScore.value += 1;

  if(remaining === 0) {
    gameOver.value = true;
    playerOneScore.value = 0;
    playerTwoScore.value = 0;
  }

};
</script>

<template>
    <h1>{{ msg }}</h1>
    <div id="game" v-if="!gameOver">
      <div class="cardContainer">
        <div id="playerOne" class="player">
        <h4>Player One</h4>
        <div class="card">
          <img :src="cardOne?.images?.png" alt="player one card" v-if="cardOne?.images">
        </div>
      </div>
      <div id="playerTwo" class="player">
        <h4>Player Two</h4>
        <div class="card">
          <img :src="cardTwo?.images?.png" alt="player two card" v-if="cardOne?.images">
        </div>
      </div>
      </div>
      <div id="scoreBoard">
          <button @click="getCards()">DrawCards</button>
          <h4>
            Player One: {{ playerOneScore }}
            Player Two: {{ playerTwoScore }}
          </h4>      
      </div>
    </div>
    <div v-if="gameOver" class="gameOver">
      <button @click="getDeck()">Play</button></div>
    
</template>
s
<style scoped>
.cardContainer {
  display: flex;
  flex-direction: column;
  gap: 40px;
}

.player {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.card {
  color: whitesmoke;
  width: 143px;
  height: 199px;
  margin: 0 auto;
  background-color: aquamarine;
  border-radius: 10px;
}

img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }

.gameOver {
  color: red;
}
</style>
