<template>
  <div class="game">
    <Title />
    <div class="container" v-if="currentPokemon">
      <Pokemon
        :pokemon="currentPokemon"
        :isChosen="pokemonChosen"
        :isChanging="isChanging"
      />
      <div class="options">
        <SuggestionsList
          :isChanging="isChanging"
          :canClick="canClick"
          :list="suggestions"
          :current="currentPokemon"
          @success="showResult(true)"
          @error="showResult(false)"
          @block="canClick = false"
        />
      </div>
    </div>
    <StatusBar
      :score="points"
      :isChosen="pokemonChosen"
      :isPassed="passed"
      :passedList="passedList"
      @reset="reset()"
      @next="showNext()"
    />
  </div>
</template>

<script>
import pokemons from '@/data/pokemons.json';
import SuggestionsList from '@/components/SuggestionsList.vue';
import Title from '@/components/Title.vue';
import Pokemon from '@/components/Pokemon.vue';
import StatusBar from '@/components/StatusBar.vue';

export default {
    name: 'Game',

    mounted() {
      this.loadPokemon();
    },

    data() {
      return {
        currentPokemon: null,
        passedPokemons: [],
        suggestions: [],
        points: 0,
        isLoaded: false,
        pokemonChosen: false,
        passed: null,
        isChanging: false,
        canClick: true,
      };
    },

    components: {
      SuggestionsList,
      Title,
      Pokemon,
      StatusBar,
    },

    computed: {
      pokeList() {
        return pokemons
          .filter((poke) => !this.passedPokemons.includes(poke));
      },
      passedList() {
        return this.passedPokemons.filter(poke => poke !== this.currentPokemon);
      },
    },

    methods: {
      showResult(stage) {
        this.canClick = false;
        this.pokemonChosen = true;
        if (stage) this.points += 1;
        this.passed = stage;
      },

      showNext() {
        this.isChanging = true;
        this.pokemonChosen = false;
        this.passed = null;

        setTimeout(() => {
          this.isChanging = false;
          this.loadPokemon();
        }, 900);
      },

      reset() {
        this.points = 0;
        this.passedPokemons = [];
        this.showNext();
      },

      loadPokemon() {
        this.isLoaded = false;
        this.canClick = true;
        const randomIndex = Math.floor(Math.random()* this.pokeList.length);
        const newPokemon = this.pokeList[randomIndex];
        this.currentPokemon = newPokemon;
        this.passedPokemons.push(newPokemon);

        const list = this.pokeList
          .sort(() => 0.5 - Math.random())
          .slice(0, 3);
        list.push(this.currentPokemon);

        this.suggestions = list.sort(() => 0.5 - Math.random());

        this.isLoaded = true;
      },
    },
}
</script>

<style lang="sass" scoped>
.game
  margin: auto

.container
  display: flex
  justify-content: space-around
  flex-direction: column
  align-items: center

.hud
  margin: auto
  margin-top: 20px
  font-size: 20px
</style>