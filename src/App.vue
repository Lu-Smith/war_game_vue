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
      font-family: 'Play', sans-serif;
      letter-spacing: 1px;
}

h1 {
      font-weight: 500;
      font-size: 2.6rem;
      margin: 40px;
      color: aquamarine;
}

h4 {

}

a {
      text-decoration: none;
      color: aquamarine;
      margin-left: 5px;
}

span {
      color: aquamarine;
}

button {
      padding: 10px 15px;
      border-radius: 5px;
      border: none;
      margin-bottom: 40px;
}
</style>
