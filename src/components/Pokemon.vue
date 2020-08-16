<template>
  <div class="pokemon">
    <PokeImg
      class="poke"
      :class="{
        black: !isChosen,
        hidden: isChanging,
      }"
      :file="pokemon.img"
    />
  </div>
</template>

<script>
import PokeImg from '@/components/PokeImg.vue';

export default {
  name: 'Pokemon',

  components: {
    PokeImg,
  },

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
};
</script>

<style lang="sass" scoped>
.poke
  opacity: 1
  transition: all 0.8s

.hidden
  opacity: 0

.black
  filter: brightness(0.0)

.pokemon
  background-image: url('../assets/blurred-circle.png')
  background-position: center
  background-repeat: no-repeat
  background-size: cover
</style>