<template>
  <p>Filter Text: {{ filterText }}</p>
  <h1>Pokedex</h1>
  <input type="text" v-model="filterText" />
  <ul>
    <li v-for="(pokemon, index) in filteredPokemonList" :key="`poke-${index}`">
      #{{ pokemon.entry_number }} - {{ pokemon.pokemon_species.name }}
    </li>
  </ul>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";

const pokemonList = ref([]);
const filterText = ref("");

const filteredPokemonList = computed(() =>
  pokemonList.value.filter((pokemon) =>
    pokemon.pokemon_species.name.includes(filterText.value)
  )
);

onMounted(async () => {
  const pokeData = await fetch("https://pokeapi.co/api/v2/pokedex/2/").then(
    (response) => response.json()
  );
  pokemonList.value = pokeData.pokemon_entries;
});
</script>

<style scoped></style>
