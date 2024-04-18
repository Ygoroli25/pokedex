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
  await fetch(pokemon.url)
    .then((res) => res.json())
    .then((res) => (pokemonUrl.value = res));
  const modal = document.getElementById("cardInfo");
  modal.classList.add("abrir");

  modal.addEventListener("click", (e) => {
    if (e.target.id == "close" || e.target.id == "cardInfo") {
      modal.classList.remove("abrir");
    }
  });
};
</script>

<template>
  <div class="content-main">
    <div>
      <CardInfo
        :name="pokemonUrl?.name"
        :height="pokemonUrl?.height"
        :weight="pokemonUrl?.weight"
        :id="pokemonUrl?.id"
        :types="pokemonUrl?.types[0].type.name"
        :img="pokemonUrl?.sprites.other.dream_world.front_default"
      />
    </div>
    <div class="conteiner" id="listPokemon">
      <img
        alt="Pokedex"
        class="card-qual-pokemon"
        src="../assets/card-qual-pokemon.svg"
        height="400px"
      />
      <div class="card-procurar-pokemon">
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
  gap: 4px;
  overflow-y: scroll;
  height: 260px;
}

.list-pokemons::-webkit-scrollbar {
  width: 10px;
  background-color: #cfcfcf;
  border-radius: 4px;
}

.list-pokemons::-webkit-scrollbar-thumb {
  background-color: #6c6c6c;
  height: 40px;
  border-radius: 4px;
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
