<template>
  <img
    class="undraggable"
    :src="img"
    alt="Pokemon"
    :width="width"
  />
</template>

<script>
export default {
  name: 'PokeImg',

  props: {
    width: {
      type: Number,
      default: 250,
    },
    file: String,
  },
  computed: {
    img() {
      if (!this.file) return require('../assets/none.png');
      const url = 'https://assets.pokemon.com/assets/cms2/img/pokedex/full/';
      return `${url}/${this.file}`;
    },
  },
};
</script>
