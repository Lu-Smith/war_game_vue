<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';

defineProps<{
  msg: string
}>();

function translateCards(value:string) {
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
const valueOne = parseInt(translateCards((cardOne.value as any)?.value));
  const valueTwo = parseInt(translateCards((cardTwo.value as any)?.value));
  if(valueOne > valueTwo) playerOneScore.value += 1;
  if(valueOne < valueTwo) playerTwoScore.value += 1;

  if(remaining === 0) {
    gameOver.value = true;
    playerOneScore.value = 0;
    playerTwoScore.value = 0;
    deckId.value = null;
  }

};
</script>

<template>
    <h1>
      {{ msg }}
    </h1>
    <div id="game" v-if="!gameOver">
      <div class="cardContainer">
        <div id="playerOne" class="player">
        <h4>Player <span>One</span></h4>
        <div class="card">
          <img :src="(cardOne as any)?.images?.png" alt="player one card" v-if="(cardOne as any)?.images">
        </div>
      </div>
      <div id="playerTwo" class="player">
        <h4>Player <span>Two</span></h4>
        <div class="card">
          <img :src="(cardTwo as any)?.images?.png" alt="player two card" v-if="(cardTwo as any)?.images">
        </div>
      </div>
      </div>
      <div id="scoreBoard">
          <button @click="getCards()">Draw Cards</button>
          <h4 class="scoreBoardPlayers">
            <div>Player <span>One</span>: {{ playerOneScore }}</div>
            <div>Player <span>Two</span>: {{ playerTwoScore }}</div>
          </h4>      
      </div>
    </div>
    <h4 v-if="playerOneScore > playerTwoScore && !gameOver">Player <span>One</span> is winning!</h4>
    <h4 v-if="playerOneScore < playerTwoScore &&  !gameOver">Player <span>Two</span> is winning!</h4>
    <h4 v-if="playerOneScore === playerTwoScore &&  !gameOver">Tie!</h4>
    <div v-if="gameOver" class="gameOver">
      <button @click="getDeck()">Play</button>
    </div>
    
</template>
s
<style scoped>
.cardContainer {
  display: flex;
  flex-direction: column;
  gap: 40px;
  margin-bottom: 40px;
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
  transition: 200ms all ease-in-out;
}

.card:hover {
    transform: scale(0.8);
}

img {
    width: 100%;
    height: 100%;
    object-fit: contain;
}

.scoreBoardPlayers {
  display: flex;
  flex-direction: row;
  gap: 20px;
  margin-bottom: 40px;
}

.gameOver {
  color: red;
}
</style>
