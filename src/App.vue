<script>
import AppMain from './components/AppMain.vue';
//* Import the page of data store
import { store } from './assets/data/store.js'

export default {
  components: { AppMain },
  data() {
    return {
      store,
    }
  }
}
</script>

<template>
  <AppMain :pokemons="store.pokemons" />
</template>

<style lang="scss">
@use './assets/scss/style.scss' as *;

body {
  background-image: url('./assets/img/pokedex-bg.jpg');
  background-position: center;
}
</style>