<script setup>
const pokemon = defineProps(["name","urlImgPokemon", "id", "types"]);


</script>

<template>
  <div class="card-list" >
    <img :src="urlImgPokemon" alt="" width="80px" height=" 60px">
  <p>#{{ pokemon.id }} {{ pokemon.name }}</p>
  <p>{{ pokemon.types }}</p>
</div>
</template>


<style>

.card-list{
display: flex;
padding: 8px;
border: 1px solid #c6c6c6;
border-radius: 4px;
gap: 8px;
background-color: #fff;
text-transform: capitalize;
margin: 4px;
cursor: pointer;
}
</style>