<template>
  <div class="home">
    <h1>Pokemon List</h1>
    <div class="poke-container">
      <div v-for="(pokemon, i) in advData" :key="i" class="poke-card">
        <img :src="pokemon.sprites.front_default" alt="sprites" />
        <h2>{{ pokemon.name }}</h2>
        <div class="poke-types">
          <div v-for="(type, j) in pokemon.types" :key="j">
            <a :href="type.type.url" target="_blank">
              {{ type.type.name }}
            </a>
          </div>
        </div>
      </div>
    </div>
    <!--<img alt="Vue logo" src="../assets/logo.png" />-->

    <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
  </div>
</template>

<script setup>
// @ is an alias to /src
import { ref } from "vue";

const data = ref([]);
const advData = ref([]);
const error = ref(null);

fetch("https://pokeapi.co/api/v2/pokemon")
  .then(async (res) => {
    data.value = await res.json();
    let promises = data.value.results.map((result) => {
      return fetch(result.url);
    });
    Promise.all(promises).then(async (response) => {
      response.forEach(async (item) => {
        const json = await item.json();
        advData.value.push(json);
      });
    });
  })
  .catch((err) => (error.value = err));
</script>

<style scoped>
.home {
  max-width: 58rem;
  margin: 0 auto;
}
.poke-container {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 20px;
}
.poke-card {
  border: 1px solid green;
  padding: 20px;
  border-radius: 12px;
}
.poke-card h2 {
  text-transform: capitalize;
}
.poke-types {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 6px;
}
</style>
