<template>
  <div v-if="visible" class="modal modal-lg fade show d-block" tabindex="-1" style="background: rgba(0, 0, 0, 0.5);"
    @click.self="closeModal">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">{{ capitalizeFirstLetter(pokemon?.name) || 'Modal' }}</h5>
          <button type="button" class="btn-close" aria-label="Close" @click="closeModal"></button>
        </div>
        <div class="modal-body" v-if="pokemon">
          <div class="text-center">
            <img :src="pokemon.sprites.front_default" :alt="pokemon.name" width="30%">
          </div>
          <div class="d-flex justify-content-center mb-4">
            <span v-for="(type, index) in pokemon.types" :key="index"
              :class="['p-2 ms-2', getTypeClass(type.type.name)]">{{ type.type.name }} </span>
          </div>
          <hr class="my-4 bg-dark">
          <div class="d-flex justify-content-center">
            <span class="bg-gainsboro p-2">Altura: {{ pokemon.height * 2.54 }} cm</span>
            <span class="bg-gainsboro p-2 ms-2">Peso: {{ pokemon.weight * 0.453592 }} kg</span>
          </div>
          <h3>Moves</h3>
          <table class="table">
            <thead>
              <tr>
                <th scope="col">Level</th>
                <th scope="col">Name</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in filter_moves(pokemon)" :key="item.move.name">
                <td>{{ get_move_level(item) }}</td>
                <td>{{ item.move.name }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" @click="closeModal">Fechar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    visible: Boolean,
    pokemon: Object
  },
  methods: {
    getTypeClass(type) {
      switch (type) {
        case 'fire':
          return 'bg-danger text-white';
        case 'water':
          return 'bg-primary text-white';
        case 'grass':
          return 'bg-success text-white';
        case 'electric':
          return 'bg-warning text-dark';
        case 'ice':
          return 'bg-info text-dark';
        case 'fighting':
          return 'bg-dark text-white';
        case 'poison':
          return 'bg-purple text-white'; // Classe personalizada
        case 'ground':
          return 'bg-brown text-white'; // Classe personalizada
        default:
          return 'bg-gainsboro text-dark'; // Classe padrão
      }
    },
    closeModal() {
      this.$emit('update:visible', false);
    },
    capitalizeFirstLetter(string) {
      if (!string) return '';
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    get_move_level(move) {
      for(let version of move.version_group_details) {
        if (version.version_group.name == "sword-shield" && version.move_learn_method.name == "level-up"){
          return version.level_learned_at;
        }
      }
    },
    filter_moves(pokemon) {
      return pokemon.moves.filter((item) => {
        let include = false;
        for (let version of item.version_group_details) {
          if (version.version_group.name == "sword-shield" && version.move_learn_method.name == "level-up") {
            include = true;
          }
        }
        return include;
      })
    }
  }
};
</script>

<style scoped>
.modal-dialog {
  margin: 1.75rem auto;
}
</style>