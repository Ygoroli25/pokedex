<script setup>
const pokemon = defineProps([
  "name",
  "urlImgPokemon",
  "id",
  "height",
  "weight",
  "types",
  "img"
]);
</script>

<template>
  <div id="cardInfo" class="card-info">
    <div>
      <section class="info-main">
        <img
          class="img-bg"
          src="../assets/bg-amarela.jpg"
          alt=""
          width="100%"
          height="50px"
        />
        <img
          class="img-pokemon-main"
          :src="pokemon.img"
          alt="pokemon"
          width="100px"
          height="100px"
        />
        <div class="info-conteiner">
          <section class="info-name">
            <p>#{{ pokemon.id }}</p>
            <h2>{{ pokemon.name }}</h2>
            <div class="info-dimensions">
              <p>Heigth: {{ pokemon.height / 10 }}m</p>
              <p>Weight: {{ pokemon.weight / 10 }}kg</p>
            </div>
          </section>
          <section class="info-types"> 
            <div>
              <p>{{ types }}</p>
            </div>
          </section>
          <hr />
          <section class="info-status">
            <h4>Status</h4>
            <span class="status">
              <div><p>{{ pokemon.statsName }}: x</p></div>            
            </span>
          </section>
          <hr />
          <section class="info-attacks">
            <h4>Movimentos de Ataque</h4>
            <div class="attacks">
              <p>Ataque X</p>
            </div>
          </section>
        </div>
      </section>
    </div>
    <div>
      <section class="info-secondary">
        <button class="close">
          <img id="close" src="../assets/close.png" alt="" width="40px" />
        </button>
        <section class="info-sprites">
          <div class="sprite">
            <img src="../assets/025.png" alt="sprites" width="60px" />
          </div>
          <div class="sprite">
            <img src="../assets/025.png" alt="sprites" width="60px" />
          </div>
          <div class="sprite">
            <img src="../assets/025.png" alt="sprites" width="60px" />
          </div>
          <div class="sprite">
            <img src="../assets/025.png" alt="sprites" width="60px" />
          </div>
          <div class="sprite">
            <img src="../assets/025.png" alt="sprites" width="60px" />
          </div>
        </section>
        <hr />
        <section class="info-indices">
          <h4>Games indices</h4>
          <span class="indices">
            <div><p>HP: x</p></div>
            <div><p>CP: x</p></div>
            <div><p>Attack: x</p></div>
            <div><p>Defence: x</p></div>
          </span>
        </section>
        <hr />
        <section class="info-evolutions">
          <h4>Evoluções</h4>
          <span class="evolution">
            <div>
              <img src="../assets/025.png" alt="Evolução" width="100px" />
              <p>Nome</p>
            </div>
          </span>
        </section>
      </section>
    </div>
  </div>
</template>

<style>
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
  gap: 10px;
}

.img-pokemon-main {
  position: absolute;
  right: 10px;
  top: 10px;
}
.info-conteiner {
  display: flex;
  flex-direction: column;
  gap: 10px;
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

.info-types,
.status,
.indices {
  display: flex;
  font-size: 12px;
  gap: 8px;
}

.info-types div {
  padding: 4px 8px;
  background-color: #141414;
  border-radius: 4px;
}

.info-status div,
.info-indices div {
  margin-top: 8px;
  padding: 4px 8px;
  background-color: #141414;
  border-radius: 4px;
}

.attacks {
  font-size: 14px;
  color: #cccccc;
  padding: 8px;
}

.info-sprites {
  display: flex;
  justify-content: space-evenly;
  flex-wrap: wrap;
  gap: 10px;
}

.evolution {
  display: flex;
  gap: 20px;
}
.evolution div {
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
</style>
