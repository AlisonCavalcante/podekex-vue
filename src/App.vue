<template>
  <div id="pokedex">
    <div class="text-center">
      <img :src="require('./assets/pokedex.png')" alt="">
    </div>
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
        @click="openModal(pokemon)"
      />
    </div>
    
    <!-- Modal -->
    <PokemonModal 
      :visible.sync="showModal"
      :pokemon="selectedPokemon"
    />
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';
import PokemonModal from './components/PokemonModal.vue';

export default {
  name: 'App',
  
  components: {
    PokemonCard,
    PokemonModal
  },

  data() {
    return {
      pokemons: [],
      searchQuery: '',
      selectedPokemon: null,
      showModal: false
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

  methods: {
    get_id(pokemon) {
      return Number(pokemon.url.split("/")[6]);
    },
    openModal(pokemon) {
      let idPokemon = null
      idPokemon = this.get_id(pokemon);
      axios.get(`https://pokeapi.co/api/v2/pokemon/${idPokemon}`).then((response) => {
      this.selectedPokemon = response.data;
      this.showModal = true;
      })
    },
   
  }
};
</script>

<style lang="scss" scoped>
/* Estilos adicionais, se necessário */
</style>
