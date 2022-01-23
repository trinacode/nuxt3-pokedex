<template>
  <div class="page">
    <div class="header">
      <div class="row centerElements">
        <h1>Pokedex</h1>
      </div>
      <div class="row centerElements">

        <form @submit.prevent="searchPokemon(pokemonInput)">
          <input v-model="pokemonInput" placeholder="pokemon name">
          <button>Go!</button>
        </form>

      </div>
    </div>

    <div class="pokemonGrid centerElements">
      <div v-for="pokemon in pokemonsList" :key="pokemon.name" class="card pokemonCard">
        <router-link :to="`/pokemonDetails?name=${pokemon.name}&id=${pokemon.id}`">
          <div class="row imgContainer centerElements">
            <img :src="pokemon.spriteUrl" alt="pk_img">
          </div>
          <div class="row centerElements">
            <label class="centerElements">{{ pokemon.name }}</label>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script setup>

const { data : getPokemons } = await useFetch('https://pokeapi.co/api/v2/pokemon?limit=100', {transform: (response) => response.results})

let pokemonsList = ref(getPokemons._rawValue)
const getPokemonId = async (pokemonName) => {
  const pokemonId = await useFetch(`https://pokeapi.co/api/v2/pokemon/${pokemonName}`, { transform: (response) => response.id});
  return pokemonId.data._value;
}

const addPokemonIdAndSpriteUrl = () => {
  pokemonsList.value.map(async (pk) => {
    pk.id = await getPokemonId(pk.name)
    pk.spriteUrl = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pk.id}.png`;
  })
}

const pokemonInput = ref('')

const searchPokemon = (pokemonInput) => {
  pokemonsList.value = getPokemons._rawValue
  pokemonsList.value = pokemonsList.value.filter(pk => pk.name.includes(pokemonInput.toLowerCase()))
}

addPokemonIdAndSpriteUrl()

</script>

<style scoped>
.page {
  background: #77A1D3;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to top, #E684AE, #79CBCA, #77A1D3);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to top, #E684AE, #79CBCA, #77A1D3); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  height: 100%;
  width: 100%;
}

.header {
  height: 15vh;
}

.centerElements {
  display: flex;
  justify-content: center;
  align-items: center;
}

.pokemonGrid {
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(auto-fill, 200px);
  grid-gap: 10px;
  grid-auto-rows: minmax(100px, auto);
  wrap-option: wrap;
}

.pokemonCard {
  width: 200px;
  height: 200px;
  border-radius: 15px;
  background-color: aliceblue;
  position: relative;
}

.pokemonCard .imgContainer {
  height: 70%;
}

.pokemonCard label {
  color: #E684AE;
  font-family: "Andale Mono", sans-serif;
  font-size: 22px;
  text-transform: capitalize;
  position: absolute;
  bottom: 0;
  height: 30%;
  width: 100%;
  background: #f0f8ff;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to top, #f8b500, #f0f8ff);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to top, #f8b500, #f0f8ff); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  border-bottom-left-radius: 15px 15px;
  border-bottom-right-radius: 15px 15px;
}


</style>