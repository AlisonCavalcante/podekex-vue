<template>
    <div v-if="visible" class="modal modal-lg fade show d-block" tabindex="-1" style="background: rgba(0, 0, 0, 0.5);"  @click.self="closeModal">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">{{ capitalizeFirstLetter(pokemon?.name) || 'Modal' }}</h5>
            <button type="button" class="btn-close" aria-label="Close" @click="closeModal"></button>
          </div>
          <div class="modal-body"  v-if="pokemon">
            <div class="text-center">
                <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`" :alt="pokemon.name" width="50%">
            </div>
            <!-- <p>{{ pokemon }}</p> -->
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
      closeModal() {
        this.$emit('update:visible', false);
      },
      capitalizeFirstLetter(string) {
      if (!string) return '';
      return string.charAt(0).toUpperCase() + string.slice(1);
      },
    }
  };
  </script>
  
  <style scoped>
  .modal-dialog {
    margin: 1.75rem auto;
  }
  </style>
  