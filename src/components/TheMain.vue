<script setup>
import { onMounted, reactive, ref, computed } from "vue";

let urlImgPokemon = ref(
  "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/"
);
let pokemonUrl = reactive(ref());
let pokemons = reactive(ref());
let searchPokemon = ref("");
let species = reactive(ref());

onMounted(() => {
  fetch("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
    .then((res) => res.json())
    .then((res) => (pokemons.value = res.results));
});

const filterPokemon = computed(() => {
  if (pokemons.value && searchPokemon.value) {
    return pokemons.value.filter(
      (pokemon) =>
        pokemon.url.split("/")[6].includes(searchPokemon.value) ||
        pokemon.name.includes(searchPokemon.value.toLowerCase())
    );
  }
  return pokemons.value;
});

const selectPokemon = async (pokemon) => {
  await fetch(pokemon.url)
    .then((res) => res.json())
    .then((res) => (pokemonUrl.value = res));

  fetch("https://pokeapi.co/api/v2/pokemon-species/" + pokemonUrl.value.id)
    .then((res) => res.json())
    .then((res) => (species.value = res));

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
  <!--Card with Pokemon information-->
  <div class="content-main">
    <section class="card-pokemon">
      <div id="cardInfo" class="card-info">
        <div>
          <section class="info-main">
            <img
              class="img-bg"
              src="../assets/imgs/background-banner-red.jpg"
              alt="Cor do Tipo"
            />
            <img
              class="img-pokemon-main"
              :src="pokemonUrl?.sprites.other.dream_world.front_default"
              alt="pokemon"
            />
            <div class="info-conteiner">
              <section class="info-name">
                <p>#{{ pokemonUrl?.id }}</p>
                <h2>{{ pokemonUrl?.name }}</h2>
                <div class="info-dimensions">
                  <p>Heigth: {{ pokemonUrl?.height / 10 }}m</p>
                  <p>Weight: {{ pokemonUrl?.weight / 10 }}kg</p>
                </div>
              </section>
              <section class="info-types">
                <div
                  v-for="(type, index) in pokemonUrl?.types"
                  :key="index"
                  :class="type.type.name"
                >
                  <p>{{ type.type.name }}</p>
                </div>
              </section>
              <hr />
              <section class="info-status">
                <h4>Status</h4>
                <span class="status">
                  <div v-for="(stat, index) in pokemonUrl?.stats" :key="index">
                    <p>{{ stat.stat.name }}: {{ stat.base_stat }}</p>
                  </div>
                </span>
              </section>
              <hr />
              <section class="info-attacks">
                <h4>Movimentos de Ataque</h4>
                <span class="attacks">
                  <div
                    v-for="(ability, index) in pokemonUrl?.abilities"
                    :key="index"
                  >
                    <p>{{ ability.ability.name }}</p>
                  </div>
                </span>
              </section>
            </div>
          </section>
        </div>
        <div>
          <section class="info-secondary">
            <button class="close">
              <img
                id="close"
                src="../assets/imgs/close.png"
                alt=""
                width="40px"
              />
            </button>
            <section class="info-indices">
              <h4>Games indices</h4>
              <span class="indices">
                <div
                  v-for="(version, index) in pokemonUrl?.game_indices"
                  :key="index"
                >
                  <p>{{ version.version.name }}</p>
                </div>
              </span>
            </section>
            <hr />
            <section class="info-evolutions">
              <h4>Evoluções</h4>
              <span class="evolution">
                <div
                  v-for="evolution in species?.evolution_chain"
                  :key="evolution"
                >
                  <img src="../assets/025.png" alt="Evolução" width="80px" />
                  
                  <p>{{ evolution.split("/")[6] }}</p>
                </div>
              </span>
            </section>
          </section>
        </div>
        <section class="info-sprites">
          <div
            class="sprite"
            v-for="(sprites, index) in pokemonUrl?.sprites"
            :key="index"
          >
            <img
              v-if="sprites && !undefined && sprites && sprites[0]"
              :src="sprites"
              alt="sprites"
              width="70px"
            />
          </div>
        </section>
      </div>
    </section>

    <!--Cards with Pokemon search-->
    <div class="conteiner" id="listPokemon">
      <img
        alt="Pokedex"
        class="card-qual-pokemon"
        src="../assets/imgs/card-qual-pokemon.svg"
        height="400px"
      />
      <div class="card-procurar-pokemon">
        <h2>Procurando um pokémon?</h2>
        <input
          v-model="searchPokemon"
          class="search"
          type="text"
          id="searchPokemon"
          placeholder="Procurar por nome ou ID..."
        />
        <div class="list-pokemons">
          <div
            class="card-list"
            v-for="pokemon in filterPokemon"
            :key="pokemon.name"
            @click="selectPokemon(pokemon)"
          >
            <img
              :src="urlImgPokemon + pokemon.url.split('/')[6] + '.svg'"
              alt=""
              width="80px"
              height=" 60px"
            />
            <div>
              <p>#{{ pokemon.url.split("/")[6] }}</p>
              <h3>{{ pokemon.name }}</h3>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
@import "../assets/base.css";

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

/* card search Pokemon */
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

.card-list {
  display: flex;
  align-items: center;
  max-height: 100px;
  padding: 8px;
  border: 1px solid #c6c6c6;
  border-radius: 4px;
  gap: 8px;
  background-color: #fff;
  text-transform: capitalize;
  margin: 4px;
  cursor: pointer;
}

.search {
  width: 100%;
  height: 35px;
  padding: 0 8px;
  border-radius: 4px;
  border: 1px solid #c6c6c6;
  margin: 8px 0 20px;
}

/* card Pokemon information */
.card-info {
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.4);
  display: none;
  justify-content: center;
  align-items: center;
  z-index: 1;
  text-transform: capitalize;
}

.card-info.abrir {
  display: flex;
}

.info-main {
  border-radius: 20px 0 0 20px;
  background-color: #3d3d3d;
  width: 320px;
  height: 400px;
  position: relative;
  color: #fff;
}
.img-bg {
  border-radius: 20px 0 0 0;
  width: 100%;
  height: 40px;
}

.info-secondary {
  padding: 10px 20px 20px;
  width: 400px;
  height: 400px;
  max-height: 400px;
  background-color: #666666;
  border-radius: 0 20px 20px 0;
  flex: 1;
  color: #fff;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.img-pokemon-main {
  position: absolute;
  right: -20px;
  top: -60px;
  width: 160px;
  height: 160px;
}
.info-conteiner {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 10px 20px;
}

.info-name p {
  color: #cccccc;
  margin-bottom: 8px;
  font-weight: 400;
  font-size: 14px;
}
.info-dimensions {
  display: flex;
  gap: 20px;
}

.info-types {
  display: flex;
  font-size: 14px;
  gap: 8px;
}

.info-types div {
  padding: 4px 16px;
  border-radius: 4px;
}

.status,
.indices {
  font-size: 10px;
  gap: 4px;
  display: flex;
  flex-wrap: wrap;
  margin-top: 8px;
}

.info-status div,
.info-indices div {
  padding: 4px 8px;
  background-color: #141414;
  border-radius: 4px;
}

.attacks {
  margin-top: 8px;
  display: flex;
  flex-wrap: wrap;
  font-size: 14px;
  color: #cccccc;
  gap: 4px;
}

.attacks div {
  border: 1px solid #cccccc;
  padding: 4px 8px;
  border-radius: 4px;
}

.info-sprites {
  max-height: 400px;
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  overflow-x: hidden;
  overflow-y: scroll;
  gap: 0px;
}

.info-sprites::-webkit-scrollbar {
  width: 10px;
  background-color: #cfcfcf;
  border-radius: 4px;
}

.info-sprites::-webkit-scrollbar-thumb {
  background-color: #666666;
  border-radius: 4px;
}

.sprite {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #3d3d3d;
  border-radius: 5px;
  margin: 4px;
}

.sprite img {
  padding: 8px;
  width: 100px;
}

.evolution {
  display: flex;
  gap: 20px;
}
.evolution div {
  margin-top: 10px;
  padding: 10px;
  background-color: #3d3d3d;
  border-radius: 4px;
  text-align: center;
}

.close {
  width: 45px;
  background-color: transparent;
  box-shadow: none;
  text-align: center;
  align-self: flex-end;
  border: 0;
  cursor: pointer;
}

/* Color Types */
.normal {
  background-color: var(--normal);
}
.fire {
  background-color: var(--fire);
}
.water {
  background-color: var(--water);
}
.grass {
  background-color: var(--grass);
}
.electric {
  background-color: var(--electric);
}
.ice {
  background-color: var(--ice);
}
.fighting {
  background-color: var(--fighting);
}
.poison {
  background-color: var(--poison);
}
.ground {
  background-color: var(--ground);
}
.flying {
  background-color: var(--flying);
}
.psychic {
  background-color: var(--psychic);
}
.bug {
  background-color: var(--bug);
}
.rock {
  background-color: var(--rock);
}
.ghost {
  background-color: var(--ghost);
}
.dark {
  background-color: var(--dark);
}
.dragon {
  background-color: var(--dragon);
}
.steel {
  background-color: var(--steel);
}
.fairy {
  background-color: var(--fairy);
}
</style>
