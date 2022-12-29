<script setup lang="ts">
import { ref } from "vue";

const pokemons = ref<any[]>([]);
const id = ref({});
const searchQuery = ref("");
const queriedPokemonsList = ref<any[]>([]);
const queriedPokemons = () => {
  if (searchQuery.value === "") {
    queriedPokemonsList.value.length = 0;
    return null;
  }
  queriedPokemonsList.value.length = 0;
  pokemons.value.map((element) => {
    if (element.name.includes(searchQuery.value)) {
      queriedPokemonsList.value.push(element);
    }
  });
};

fetch("https://pokeapi.co/api/v2/pokemon")
  .then((res) => res.json())
  .then((data) => {
    pokemons.value = data.results;
  });
</script>

<template>
  <main>
    <input
      type="text"
      placeholder="Pokemon name here..."
      v-model.trim="searchQuery"
      @input="queriedPokemons()"
    />
    <div class="wrapper">
      <div
        class="single-pokemon"
        v-for="(pokemon, id) in queriedPokemonsList"
        :key="id"
      >
        <router-link
          :to="`/${pokemon.name}`"
          class="router"
        >
          {{ pokemon.name }}
        </router-link>
      </div>
    </div>
  </main>
</template>
