<template>
    <div class="pokemon-card">
        <div class="pokemon-container-card">
            <span class="pokemon-name">
                {{ get_name(pokemon) }}
            </span>
            <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${get_id(
                pokemon
            )}.png`" :alt="pokemon.name" width="50%">
        </div>
    </div>
</template>

<script>
export default {
    name: 'PokemonCard',
    props: {
        pokemon: {
            type: Object,
            required: true
        }
    },
    methods: {
        get_id(pokemon) {
            return Number(pokemon.url.split("/")[6]);
        },
        get_name(pokemon) {
            return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
        }
    },
}
</script>

<style lang="scss" scoped></style>