<script setup></script>

<template>
  <div class="header">
    <div class="conteiner-header">
      <img
        alt="Pokedex"
        src="../assets/imgs/Pok-dex-15-04-2024.png"
        width="200px"
        height="70px"
      />
    </div>
    <img
      alt="Pokedex"
      class="bola-pokebola"
      src="../assets/imgs/bola_pokebola.svg"
      width="60px"
      height="60px"
    />
  </div>
</template>

<style scoped>
.header {
  background-color: red;
  width: 100%;
  position: relative;
  height: 72px;

}

.conteiner-header {
  width: 100%;
  max-width: 1024px;
  display: flex;
  justify-content: space-between;
  margin: 0 auto;
  align-items: center;
}

.bola-pokebola {
  position: absolute;
  bottom: -30px;
  left: calc(50% - 30px);
}

select{
  background-color: #c8c8c8;
}

select {
  padding: 8px;
  background-color: transparent;
  color: #fff;
  border-radius: 4px;
  border-color: #fff;
}

option{
  background-color: #c8c8c8;
}
</style>
