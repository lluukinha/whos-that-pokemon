<template>
  <div id="app">
    <Game />
  </div>
</template>

<script>
import Game from '@/views/Game.vue';

export default {
  name: 'App',
  components: {
    Game,
  },
};
</script>

<style>
html, body {
  height: 100%;
}

body {
  background: radial-gradient(circle, rgba(255,0,0,1) 0%, rgba(121,9,9,1) 66%, rgb(128 0 0) 100%) no-repeat center center;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
}

.undraggable {
  user-select: none;
  -moz-user-select: none;
  -webkit-user-drag: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
</style>
