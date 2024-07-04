<template>
  <div class="home">
    <h1>Pokemon List</h1>
    <div v-if="advData.length" class="poke-container">
      <PokeCard v-for="(pokemon, i) in advData" :key="i" :pokemon="pokemon" />
    </div>
    <div v-else-if="isLoading">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="1em"
        height="1em"
        viewBox="0 0 24 24"
      >
        <path
          fill="currentColor"
          d="M12,1A11,11,0,1,0,23,12,11,11,0,0,0,12,1Zm0,19a8,8,0,1,1,8-8A8,8,0,0,1,12,20Z"
          opacity="0.25"
        />
        <circle cx="12" cy="2.5" r="1.5" fill="currentColor">
          <animateTransform
            attributeName="transform"
            dur="0.75s"
            repeatCount="indefinite"
            type="rotate"
            values="0 12 12;360 12 12"
          />
        </circle>
      </svg>
    </div>
  </div>
</template>

<script setup>
// @ is an alias to /src
import { ref, onMounted } from "vue";
import PokeCard from "@/components/PokeCard.vue";
import { useRouter } from "vue-router";
const data = ref([]);
const advData = ref([]);
const error = ref(null);
const router = useRouter();
const isLoading = ref(true);
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
  .catch((err) => (error.value = err))
  .finally(() => {
    isLoading.value = false;
  });

onMounted(() => {
  if (!localStorage.getItem("user")) {
    router.push("/login");
  }
});
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
