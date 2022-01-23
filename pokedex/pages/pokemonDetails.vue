<template>
  <div class="row centerElements">
    <div class="pokemonCard">
      <div class="abilities">
        <h2>Abilities</h2>
        <div v-for="ability in abilities" class="ability">
          -{{ ability.ability.name }}
        </div>
      </div>

      <div class="row imgContainer centerElements">
        <img :src="pokemonSprite" alt="pk_img">
      </div>
      <div class="row centerElements">
        <label class="centerElements">{{ pokemonName }}</label>
      </div>
    </div>
  </div>

</template>

<script setup>
  const route = useRoute()
  const pokemonName = route.query.name
  const pokemonID = route.query.id
  const { data : pkInfo } = await useFetch(`https://pokeapi.co/api/v2/pokemon/${pokemonName}`)
  const abilities = pkInfo.value.abilities
  const pokemonWeight = pkInfo.value.weight
  const pokemonSprite = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${pokemonID}.png`
</script>

<style scoped>
.centerElements {
  display: flex;
  justify-content: center;
  align-items: center;
}

.pokemonCard {
  width: 600px;
  height: 800px;
  border-radius: 15px;
  background-color: aliceblue;
  position: relative;
}

.pokemonCard .imgContainer {
  height: 70%;
  position: absolute;
  left: 10%;
  z-index: 10;
}

.pokemonCard label {
  color: #E684AE;
  font-family: "Andale Mono", sans-serif;
  font-size: 50px;
  text-transform: capitalize;
  position: absolute;
  bottom: 0;
  height: 20%;
  width: 100%;
  background: #f0f8ff;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to top, #f8b500, #f0f8ff);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to top, #f8b500, #f0f8ff); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  border-bottom-left-radius: 15px 15px;
  border-bottom-right-radius: 15px 15px;
}

.abilities {
  padding-inline: 20px;
}

.ability {
  color: #671f3b;
  font-family: "Andale Mono", sans-serif;
  font-size: 32px;
  text-transform: capitalize;
}

.pokemonCard h2 {
  color: #671f3b;
  font-family: Dialog, sans-serif;
  font-style: italic;
}
</style>