<template>
  <table>
    <thead>
      <tr>
        <th colspan="2">PASSED LIST</th>
      </tr>
    </thead>
    <tbody>
      <tr v-if="list.length === 0">
        <td colspan="2">0 POKEMONS PASSED</td>
      </tr>
      <tr v-for="pokemon in list" :key="pokemon.numero_dex">
        <td>{{ pokemon.nome }}</td>
        <td>
          <PokeImg :width="50" :file="pokemon.img" />
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import PokeImg from '@/components/PokeImg.vue';

export default {
  name: 'PassedList',

  components: {
    PokeImg,
  },

  props: {
    list: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style lang="sass" scoped>
table
  margin-top: 20px
  margin-bottom: 20px
  tbody
    height: 50vh
    display: block
    overflow-x: hidden
    overflow-y: auto

table>tbody>tr>td, table>tbody>tr>th, table>tfoot>tr>td, table>tfoot>tr>th, table>thead>tr>td, table>thead>tr>th
  padding: .75rem
  vertical-align: center
  border: 1px solid #dee2e6
</style>