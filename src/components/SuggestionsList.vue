<template>
  <div>
    <h1 class="pokemon-name undraggable" :class="{ showName : showName }">
      {{ current.nome }}
    </h1>
    <ul>
      <li
        v-for="suggestion in list"
        :key="suggestion.numero_dex"
        class="undraggable"
        :class="{
          selected: checkSelected(suggestion),
          success: showRightSuggestion(suggestion),
          error: checkError(suggestion),
          unclickable: !canClick
        }"
        @click="choose(suggestion)"
      >
        {{ suggestion.nome }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'SuggestionsList',

  watch: {
    current() {
      this.pokemonChosen = null;
      this.countDown = 1;
    },
  },

  data() {
    return {
      pokemonChosen: null,
      countDown: 1,
      showCountdown: false,
    };
  },

  props: {
    list: {
      type: Array,
      required: true,
    },
    current: {
      type: Object,
      required: true,
    },
    canClick: {
      type: Boolean,
      required: true,
    },
    isChanging: {
      type: Boolean,
      required: true,
    }
  },

  computed: {
    showName() {
      return !this.isChanging && this.countDown === 0;
    },
  },

  methods: {
    runTimer(pokemon) {
      if(this.countDown > 0) {
        this.showCountdown = true;
        setTimeout(() => {
          this.countDown -= 1;
          this.runTimer(pokemon);
        }, 1000);
        return;
      }

      this.showCountdown = false;
      if (this.checkSuccess(pokemon)) this.$emit('success');
      if (this.checkError(pokemon)) this.$emit('error');
    },

    choose(pokemon) {
      if (!this.canClick) return;
      this.$emit('block');
      this.pokemonChosen = pokemon;
      this.runTimer(pokemon);
    },

    checkSuccess(pokemon) {
      return this.countDown === 0
        && this.pokemonChosen === pokemon
        && this.pokemonChosen === this.current;
    },

    showRightSuggestion(pokemon) {
      return this.countDown === 0 && pokemon === this.current;
    },

    checkError(pokemon) {
      return this.countDown === 0
        && this.pokemonChosen === pokemon
        && this.pokemonChosen !== this.current;
    },

    checkSelected(pokemon) {
      return this.countDown > 0 && this.pokemonChosen === pokemon;
    },
  }
};
</script>

<style lang="sass" scoped>
.countdown
  opacity: 0
  transition: all 0.8s
  margin-top: -40px

.show
  opacity: 1

ul
  min-width: 200px
  max-width: 300px
  display: -ms-flexbox
  display: flex
  -ms-flex-direction: column
  flex-direction: column
  padding-left: 0
  margin-bottom: 0
  border-radius: .25rem

  li:first-child
    border-top-left-radius: inherit
    border-top-right-radius: inherit

  li
    cursor: pointer
    position: relative
    display: block
    padding: .50rem 1.25rem
    border: 1px solid rgba(0,0,0,.125)
    transition: background-color 0.5s
    background-color: #580000
    text-shadow: 1px 1px 1px black
    &:hover
        &:not(.unclickable)
          background-color: #695642

  .unclickable
    cursor: default

  .success
    background-color: #25d825

  .error
    background-color: #ff6969

  .selected
    background-color: #695642

.pokemon-name
  margin-top: -20px
  opacity: 0
  transition: all 0.8s
  margin-bottom: 1px
  text-shadow: 2px 5px 5px #000000

.showName
  opacity: 1 !important
</style>
