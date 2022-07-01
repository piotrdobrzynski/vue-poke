<template>
  <h1>Pokemon Evolution</h1>

    <PokemonThumb v-for="(pokemon, idx) in allPokemons" :key="idx" :id="pokemon.id" :name="pokemon.name" :image="pokemon.sprites.other.dream_world.front_default" :type="pokemon.types[0].type.name"/>

</template>

<script>
import {reactive} from "vue";
import PokemonThumb from "./PokemonThumb.vue";

export default {
  name: 'App',
  components:{
    PokemonThumb
  },
  setup(){
    const state = reactive({
      allPokemons: []
    })

    fetch('https://pokeapi.co/api/v2/pokemon?limit=500')
    .then((res) => res.json())
    .then((data) => {
        data.results.forEach( async pokemon => {
          const res = await fetch(`https://pokeapi.co/api/v2/pokemon/${pokemon.name}`)
          const data =  await res.json()
          state.allPokemons.push(data)
          await state.allPokemons.sort((a, b) => a.id - b.id)
        })
    })
    return state
  }
}
</script>
