<template>
  <div id="pokedex">
    <h1 class="text--yellow">Pokedex</h1>
    <input 
    type="text"
    class="search-input"
    placeholder="Charmander"
    v-model="searchQuery"
    >
    <div id="container">
      <PokemonCard
       v-for="(pokemon, index) in filtered_pokemons"
       :key="index" 
       :pokemon="pokemon" 
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';


export default {
  name: 'App',
  
  components: {
    PokemonCard
  },

  data() {
    return {
      pokemons: [],
      searchQuery: '',
    }
  },

  mounted() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=50&offset=50").then((response) => {
      this.pokemons = response.data.results;
    })
  },
 
  computed: {
    filtered_pokemons() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.searchQuery.toLowerCase());
      })
    }
  },
};
</script>

<style lang="scss" scoped>

</style>
