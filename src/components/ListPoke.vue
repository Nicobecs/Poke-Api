<script setup>
  import axios from 'axios';
  import { ref, onMounted, computed } from 'vue';
  import CardPoke from '@/components/CardPoke.vue';

  const listPoke = ref([]);

  onMounted(async () => {
    try {
      for (let i = 1; i <= 20; i++) {
        const url = `https://pokeapi.co/api/v2/pokemon/${i}`;
        const { data } = await axios.get(url);
        //console.log(data); //ver los atributos para encontrar la ruta de la imagen
        listPoke.value.push({ ...data, esCorrecto: false });
      }
    } catch (error) {
      console.error('No se pudo atrapar un pokemon');
    }
  });

  const counter = computed(() => {
    return listPoke.value.filter((pokemon) => pokemon.esCorrecto).length;
  });

  const discovered = (namePokemon) => {
    const foundPokemon = listPoke.value.find(
      (pokemon) => pokemon.name.toLowerCase() === namePokemon
    );
    if (foundPokemon) {
      foundPokemon.esCorrecto = true;
    }
  }
</script>

<template>
  <div class="container">
    <div class="row text-center">
      <div>
        <img src="../../public/pokemon.png" alt="pokemon" class="w-50" />
      </div>
      <h1>¿Quién es este pokemon?</h1>
      <p>Pokemones encontrados: {{ counter }}</p>
      <CardPoke
        v-for="(pokemon, idx) in listPoke"
        :key="idx"
        :namePokemon="pokemon.name"
        :imageUrl="pokemon.sprites.other.dream_world.front_default"
        @send-name="discovered"
      />
    </div>
  </div>
</template>
<style scoped></style>
