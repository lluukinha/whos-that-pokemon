<template>
  <div>
    <img :src="image" />
    <h2 v-html="message" />
    <button @click="$emit('reset')">Restart Game</button>
  </div>
</template>

<script>
export default {
  name: 'EndGame',

  props: {
    score: {
      type: Number,
      required: true,
    },
  },

  computed: {
    message() {
      let msg = `Congratulations! You know all the Kanto Pokemons.<br>I'm sure you had a wonderful childhood`;
      if (this.score < 40) msg = 'Not even 40? You should study more about pokemons!';
      if (this.score > 39 && this.score < 151) msg = `${this.score}? Not bad, but i know you can get more than that.`;
      return msg;
    },
    image() {
      let img = 'oak.png';
      if (this.score < 40) img = 'madOak.png';
      if (this.score > 39 && this.score < 151) img = 'normalOak.png';
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
