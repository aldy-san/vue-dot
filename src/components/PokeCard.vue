<template>
  <div class="poke-card">
    <img :src="pokemon?.sprites?.front_default" alt="sprites" />
    <h2>{{ pokemon.name }}</h2>
    <div class="poke-types">
      <div v-for="(type, j) in pokemon.types" :key="j">
        <a :href="type.type.url" target="_blank" class="poke-type">
          {{ type.type.name }}
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    pokemon: {
      type: Object,
      default: Object,
    },
  },
};
</script>

<style scoped>
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
.poke-type {
  border: 1px solid green;
  border-radius: 20px;
  text-decoration: none;
  color: green;
  padding: 6px 14px;
}
</style>
