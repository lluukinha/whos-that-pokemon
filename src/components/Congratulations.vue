<template>
  <div>
    <img src="../assets/oak.png" />
    <h2 class="congratulations">
      Seems we have someone smarter than me here?<br>
      Congratulations! You know all the Kanto pokemons.
    </h2>
    <button @click="$emit('reset')">Restart Game</button>
  </div>
</template>

<script>
export default {
  name: 'Congratulations',
}
</script>

<style lang="sass" scoped>
h2
  text-shadow: 3px 5px 5px #000000
  margin-top: -7px
</style>
