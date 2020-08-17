<template>
  <div class="hud" ref="hud">
    <div class="changeable" @click="changeView">
      <div class="symbol" />
    </div>
    <div class="top">
      <div>Score: {{ score }}</div>
      <div>
        <button class="details-btn" @click="changeView" v-if="!showMore">DETAILS ⬆</button>
      </div>
    </div>
    <div class="middle" v-show="showMore">
      <PassedList :list="passedList" />
    </div>
    <div v-show="showMore">
      <button class="details-btn" @click="showMore = false">HIDE ↓</button>
      <button class="error" @click="restartGame()">
        RESTART GAME
      </button>
    </div>
  </div>
</template>

<script>
import PassedList from '@/components/PassedList.vue';

export default {
  name: 'StatusBar',

  watch: {
    showMore(value) {
      const el = this.$refs.hud;
      if (value) el.style.height = '90vh';
      if (!value) el.style.height = '50px';
    },
  },

  props: {
    score: {
      type: Number,
      required: true,
    },
    isChosen: {
      type: Boolean,
      required: true,
    },
    isPassed: {
      type: Boolean,
    },
    passedList: {
      type: Array,
    },
  },

  data() {
    return {
      showMore: false,
    };
  },

  components: {
    PassedList,
  },

  methods: {
    changeView() {
      this.showMore = !this.showMore;
    },
    restartGame() {
      this.showMore = false;
      this.$emit('reset');
    },
  },
};
</script>

<style lang="sass" scoped>
.changeable
  width: 100%

.hud
  position: fixed
  left: 0
  min-height: 40px
  height: auto
  bottom: 0
  width: 100%
  color: white
  background-color: #1A202C
  border-top-left-radius: 20px
  border-top-right-radius: 20px
  box-shadow: -2px 0px 4px black
  transition: all 0.5s

.top
  display: flex
  justify-content: space-around
  align-items: center
  height: 30px
  font-size: 16px

.symbol
  cursor: pointer
  width: 40px
  height: 2px
  background-color: white
  margin: auto
  margin-top: 5px
  margin-bottom: 5px
  box-shadow: 1px 1px 2px black

.middle
  display: flex;
  justify-content: center

button
  display: inline-block
  font-weight: 400
  text-align: center
  white-space: nowrap
  vertical-align: middle
  -webkit-user-select: none
  -moz-user-select: none
  -ms-user-select: none
  user-select: none
  border: 1px solid transparent
  padding: .375rem .75rem
  color: white
  padding: .25rem .5rem
  font-size: .575rem
  line-height: 1.5
  border-radius: .2rem

.error
  background-color: #ff6969

.success
  background-color: #25d825

.details-btn
  margin-right: 10px
  color: black
</style>