<template>
  <div class="game">
    <Title />
    <EndGame
      class="container"
      v-if="points === fullDex || gameOver"
      :score="points"
      @reset="reset()"
    />
    <div class="container" v-else>
      <Pokemon
        :img="pokemonImage"
        :isChosen="pokemonChosen"
        :isChanging="isChanging"
      />
      <div class="options" v-if="currentPokemon">
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
    />
  </div>
</template>

<script>
import pokemons from '@/data/pokemons.json';
import SuggestionsList from '@/components/SuggestionsList.vue';
import Title from '@/components/Title.vue';
import Pokemon from '@/components/Pokemon.vue';
import StatusBar from '@/components/StatusBar.vue';
import EndGame from '@/components/EndGame.vue';

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
        countDown: 3,
        gameOver: false,
      };
    },

    components: {
      SuggestionsList,
      Title,
      Pokemon,
      StatusBar,
      EndGame,
    },

    computed: {
      fullDex() {
        return pokemons.length;
      },
      pokeList() {
        return pokemons
          .filter((poke) => !this.passedPokemons.includes(poke));
      },
      passedList() {
        return this.passedPokemons.filter(poke => poke !== this.currentPokemon);
      },
      pokemonImage() {
        return this.currentPokemon?.img || null;
      },
    },

    methods: {
      showResult(stage) {
        this.canClick = false;
        this.pokemonChosen = true;
        if (stage) this.points += 1;
        this.passed = stage;
        this.continueToCountDown();
      },

      continueToCountDown() {
        if(this.countDown > 0) {
          setTimeout(() => {
            this.countDown -= 1;
            this.continueToCountDown();
          }, 500);
          return;
        }

        if (!this.passed) {
          setTimeout(() => {
            this.gameOver = true;
          }, 1000);
          return;
        }
        this.showNext();
      },

      showNext() {
        this.isChanging = true;
        this.pokemonChosen = false;
        this.passed = null;
        this.currentPokemon = null;
        this.countDown = 3;

        setTimeout(() => {
          this.isChanging = false;
          this.loadPokemon();
        }, 900);
      },

      reset() {
        this.gameOver = false;
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

        const suggestions = pokemons
          .filter(pokemon => pokemon !== newPokemon).sort(() => 0.5 - Math.random()).slice(0, 3);
        suggestions.push(this.currentPokemon);
        this.suggestions = suggestions.sort(() => 0.5 - Math.random());

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