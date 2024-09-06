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

      <!-- Pagination Controls -->
      <div class="pagination-controls text-center">
      <button 
        class="btn btn-primary" 
        :disabled="!prevUrl" 
        @click="prevPage"
      >
        Anterior
      </button>
      <span class="m-2">Página {{ currentPageLabel }} de {{ totalPages }}</span>
      <button 
        class="btn btn-primary" 
        :disabled="!nextUrl" 
        @click="nextPage"
      >
        Próxima
      </button>
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
      showModal: false,
      totalPokemons: 0,
      itemsPerPage: 50,
      currentPage: 1,       
      nextUrl: null,
      prevUrl: null
    }
  },

  mounted() {
    this.fetchPokemons("https://pokeapi.co/api/v2/pokemon?limit=50");
  },
 
  computed: {
    filtered_pokemons() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.searchQuery.toLowerCase());
      })
    },
    totalPages() {
      return Math.ceil(this.totalPokemons / this.itemsPerPage);
    },
    currentPageLabel() {
      return this.currentPage;
    }
  },

  methods: {
    fetchPokemons(url) {
      axios.get(url)
        .then((response) => {
          this.pokemons = response.data.results;
          this.nextUrl = response.data.next;  // URL para a próxima página
          this.prevUrl = response.data.previous;  // URL para a página anterior
          this.totalPokemons = response.data.count;
        })
        .catch((error) => {
          console.error("Failed to fetch pokemons:", error);
        });
    },
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
    nextPage() {
      if (this.nextUrl) {
        this.currentPage++;
        this.fetchPokemons(this.nextUrl);
      }
    },
    prevPage() {
      if (this.prevUrl) {
        this.currentPage--;
        this.fetchPokemons(this.prevUrl);
      }
    },
  }
};
</script>

<style lang="scss" scoped>

</style>
