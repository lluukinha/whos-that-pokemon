<template>
  <div class="pokemon">
    <img
      class="poke undraggable"
      :class="{
        black: !isChosen,
        hidden: isChanging,
      }"
      :src="imgPokemon"
      alt="Pokemon"
    />
  </div>
</template>

<script>
export default {
  name: 'Pokemon',

  props: {
    pokemon: {
      type: Object,
      required: true,
    },
    isChosen: {
      type: Boolean,
      required: true,
    },
    isChanging: {
      type: Boolean,
      required: true,
    },
  },

  computed: {
    imgPokemon() {
      const pokeImg = this.pokemon.img;
      return `https://assets.pokemon.com/assets/cms2/img/pokedex/full/${pokeImg}`;
    },
  },
};
</script>

<style lang="sass" scoped>
.poke
  max-width: 250px
  opacity: 1
  transition: all 0.8s

.hidden
  opacity: 0

.black
  filter: brightness(0.0)
</style>