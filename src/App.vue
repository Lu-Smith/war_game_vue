<script setup lang="ts">
import FooterComponent from './components/FooterComponent.vue';
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
      <div class="App">
            <HelloWorld msg="War Game" />
            <FooterComponent />
      </div>  
</template>

<style>
.App {
      color: whitesmoke;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      text-align: center;
      letter-spacing: 1px;
}

h1 {
      font-weight: 500;
      font-size: 2.6rem;
      margin: 40px;
      color: aquamarine;
      font-family: 'Play', sans-serif;
}

a {
      text-decoration: none;
      color: aquamarine;
      margin-left: 5px;
}

a:hover {
      color: rgb(40, 114, 90);
}

span {
      color: aquamarine;
}

button {
      padding: 10px 15px;
      border-radius: 5px;
      border: none;
      margin-bottom: 40px;
      transition: 200ms all ease-in-out;
      font-family: 'Play', sans-serif;
      letter-spacing: 1px;
      font-weight: 600;
}

button:hover {
      background-color: rgb(198, 199, 197);
      transform: scale(0.95);
}
</style>
