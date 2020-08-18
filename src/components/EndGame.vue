<template>
  <div>
    <img :src="image" />
    <h2 v-html="message" />
    <button @click="$emit('reset')">Restart Game</button>
  </div>
</template>

<script>
import pokemons from '@/data/pokemons.json';

export default {
  name: 'EndGame',

  props: {
    score: {
      type: Number,
      required: true,
    },
  },

  computed: {
    fullDex() {
      return pokemons.length;
    },
    message() {
      let msg = `Congratulations! You know all Kanto Pokemons and some of Johto.<br>I bet you had a wonderful childhood. Come back later to see if i have more.`;
      if (this.score < 40) msg = 'Not even 40? You should study more about pokemons!';
      if (this.score > 39 && this.score < this.fullDex) msg = `${this.score}? Not bad, but i know you can get more than that.`;
      return msg;
    },
    image() {
      let img = 'oak.png';
      if (this.score < 40) img = 'madOak.png';
      if (this.score > 39 && this.score < this.fullDex) img = 'normalOak.png';
      return require(`../assets/${img}`);
    },
  }
}
</script>

<style lang="sass" scoped>
img
  max-width: 250px

h2
  text-shadow: 3px 5px 5px #000000
  margin-top: -7px
</style>
