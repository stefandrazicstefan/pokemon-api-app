<script setup lang="ts">
import { ref } from "vue";
import { useRoute } from "vue-router";
import { IntersectingCirclesSpinner } from "epic-spinners";

const route = useRoute();
const pokemon = ref<any>({});
const backDefault = ref("");
const frontDefault = ref("");
const types = ref<any[]>([]);
const loading = ref(true);

fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
  .then((response) => response.json())
  .then((data) => {
    pokemon.value = data;
    backDefault.value = pokemon.value.sprites.back_default;
    frontDefault.value = pokemon.value.sprites.front_default;
    types.value = pokemon.value.types;
    loading.value = false;
  });
</script>

<template>
  <intersecting-circles-spinner
    :animation-duration="1200"
    :size="70"
    class="spinner"
    v-if="loading"
  />
  <div
    class="single-pokemon-card"
    v-else
  >
    <h2>{{ pokemon.name }}</h2>
    <img
      :src="frontDefault"
      alt=""
    />
    <img
      :src="backDefault"
      alt=""
    />
    <p>Types:</p>
    <p v-for="_type in types">{{ _type.type.name }}</p>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
