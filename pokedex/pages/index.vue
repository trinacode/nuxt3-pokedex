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
      <div v-for="pokemon in pokemonsList" :key="pokemon.name" class="pokemonCard">
        <button class="addPokemonToTeamButton" @click="addPokemonToTeam(pokemon)">+</button>
        <router-link :to="`/pokemonDetails?name=${pokemon.name}&id=${pokemon.id}`">
          <div class="row imgContainer centerElements">
            <img :src="pokemon.spriteUrl" alt="pk_img">
          </div>
          <div class="row centerElements">
            <label class="centerElements pokemonCardLabel">{{ pokemon.name }}</label>
          </div>
        </router-link>
      </div>
    </div>

    <div v-if="pokeTeam" class="team centerElements">
      <button class="closeTeamButton" @click="displayPokeTeam()" v-if="pokeTeam">X</button>
      <div class="slot" v-for="(slot, index) in 5" :key="slot">
        <div v-if="pokemonTeam.length > index" class="pokemonTeamCard">
          <div class="row imgContainer centerElements">
            <img :src="pokemonTeam[index].spriteUrl" alt="pk_img">
          </div>
          <label class="pokemonCardLabel centerElements">{{pokemonTeam[index].name}}</label>
        </div>
      </div>
    </div>

    <button class="pokemonTeamButton" @click="displayPokeTeam()" v-if="!pokeTeam">Show team</button>

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

const pokeTeam = ref(false)
const pokemonTeam = ref([])

const addPokemonToTeam = (pokemon) => {
  if(pokemonTeam.value.length < 5) {
    pokemonTeam.value.push(pokemon)
  } else {
    alert('Your team is limited to 5 pokemons')
  }
}

const displayPokeTeam = () => {
  pokeTeam.value = !pokeTeam.value
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
  grid-gap: 20px;
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

.pokemonCardLabel {
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

.addPokemonToTeamButton {
  background-color: #36ab0e;
  border-radius: 15px;
  position: absolute;
  width: 30px;
  height: 30px;
  right: -10px;
  top: -10px;
  font-size: 20px;
  color: white;
}

.team {
  background: #2980B9;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to bottom, #FFFFFF, #6DD5FA);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to bottom, #FFFFFF, #6DD5FA); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  position: -webkit-sticky; /* Safari */
  position: sticky;
  bottom: 0;
  height: 200px;
  width: 60vw;
  left: 20vw;
  border-top-right-radius: 15px 15px;
  border-top-left-radius: 15px 15px;
}

.team .slot {
  display: grid;
  margin-inline: 10px;
  border: 1px solid dimgrey;
  height: 150px;
  width: 150px;
  border-radius: 15px;
}

.pokemonTeamCard {
  width: 150px;
  height: 150px;
  border-radius: 15px;
  background-color: aliceblue;
  position: relative;
}

.pokemonTeamButton {
  background: #2980B9;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to bottom, #FFFFFF, #6DD5FA, #2980B9);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to bottom, #FFFFFF, #6DD5FA, #2980B9); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  position: -webkit-sticky; /* Safari */
  position: sticky;
  bottom: 40px;
  left: 40px;
  height: 100px;
  width: 100px;
  border-radius: 50px;
}

.closeTeamButton {
  background-color: #c20b36;
  border-radius: 15px;
  position: absolute;
  width: 30px;
  height: 30px;
  right: -10px;
  top: -10px;
  font-size: 20px;
  color: white;
}

</style>