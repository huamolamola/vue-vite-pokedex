<template>
  <h1>Pokedex</h1>
  <input type="text" v-model="filterText" />
  <ul>
    <PokedexCard
      v-for="(pokemon, index) in pokemonStore.filteredList"
      :key="`poke-${index}`"
      :name="pokemon.pokemon_species.name"
      :number="pokemon.entry_number"
    />
  </ul>
</template>

<script setup>
import { reactive, ref, computed, onMounted } from "vue";
import PokedexCard from "./components/PokedexCard.vue"

const filterText = ref("");

const pokemonStore = reactive({
  list: [],
  filteredList: computed(() =>
    pokemonStore.list.filter((pokemon) =>
      pokemon.pokemon_species.name.includes(filterText.value)
    )
  ),
});

onMounted(async () => {
  const pokeData = await fetch("https://pokeapi.co/api/v2/pokedex/2/").then(
    (response) => response.json()
  );
  pokemonStore.list = pokeData.pokemon_entries;
});
</script>

<style scoped></style>
