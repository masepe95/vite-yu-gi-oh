<script>
import AppMain from './components/AppMain.vue';
//* Import the page of data store
import { store } from './assets/data/store.js'
import AppHeader from './components/AppHeader.vue';

export default {
  components: { AppMain, AppHeader },
  data() {
    return {
      store,
    }
  }
}
</script>

<template>
  <AppHeader />
  <AppMain />
</template>

<style lang="scss">
@use './assets/scss/style.scss' as *;

body {
  background-image: url('./assets/img/pokedex-bg.jpg');
  background-position: center;
}
</style>