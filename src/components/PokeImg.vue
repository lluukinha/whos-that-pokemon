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
    file: {
      type: String,
      required: true,
    },
  },
  computed: {
    img() {
      const url = 'https://assets.pokemon.com/assets/cms2/img/pokedex/full/';
      return `${url}/${this.file}`;
    },
  },
};
</script>
