<script setup>
import CardInfo from "./CardInfo.vue";
import ListPokemons from "./ListPokemons.vue";
import { onMounted, reactive, ref, computed } from "vue";

let urlImgPokemon = ref(
  "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/"
);
let pokemonUrl = reactive(ref());
let pokemons = reactive(ref());
let searchPokemon = ref("");
onMounted(() => {
  fetch("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
    .then((res) => res.json())
    .then((res) => (pokemons.value = res.results));
});

const filterPokemon = computed(() => {
  if (pokemons.value && searchPokemon.value) {
    return pokemons.value.filter((pokemon) =>
      pokemon.name.includes(searchPokemon.value.toLowerCase())
    );
  }
  return pokemons.value;
});

const selectPokemon = async (pokemon) => {
  fetch(pokemon.url)
    .then((res) => res.json())
    .then((res) => (pokemonUrl.value = res));
  console.log(pokemonUrl.value);
};
</script>

<template>
  <div class="content-main">
    <div class="conteiner">
      <img
        alt="Pokedex"
        class="card-qual-pokemon"
        src="../assets/card-qual-pokemon.svg"
        height="400px"
      />
      <div class="card-procurar-pokemon">
        <div>
      <CardInfo 
      :name="pokemonUrl?.name"
      />
    </div>
        <h2>Procurando um pokémon?</h2>
        <input
          v-model="searchPokemon"
          class="search"
          type="text"
          id="searchPokemon"
          placeholder="Procurar..."
        />
        <div class="list-pokemons">
          <ListPokemons
            v-for="pokemon in filterPokemon"
            :key="pokemon.name"
            :id="pokemon.url.split('/')[6]"
            :name="pokemon.name"
            :urlImgPokemon="urlImgPokemon + pokemon.url.split('/')[6] + '.svg'"
            @click="selectPokemon(pokemon)"
          />
        </div>
      </div>
    </div>
   
  </div>
</template>

<style>
.conteiner {
  max-width: 1024px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  flex: 1;
  align-items: center;
  align-content: center;
  gap: 40px;
  flex-wrap: wrap;
}
.content-main {
  height: calc(100vh - 72px);
  display: flex;
  width: 100vw;
  justify-content: center;
  align-items: center;
}
.card-procurar-pokemon {
  padding: 20px 40px;
  width: 400px;
  max-height: 400px;
  background-color: #f2f2f2;
  border-radius: 20px;
  flex: 1;
}

.list-pokemons {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
  overflow-y: scroll;
  height: 260px;
}

.search {
  width: 100%;
  height: 35px;
  padding: 0 8px;
  border-radius: 4px;
  border: 1px solid #c6c6c6;
  margin: 8px 0 20px;
}
</style>
