<template>
  <div class="home">
    <h1>Pokemon List</h1>
    <div v-if="advData.length" class="poke-container">
      <PokeCard v-for="(pokemon, i) in advData" :key="i" :pokemon="pokemon" />
    </div>
  </div>
</template>

<script setup>
// @ is an alias to /src
import { ref } from "vue";
import PokeCard from "@/components/PokeCard.vue";
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
</style>
