<template>
  <Navbar @section="changeSection" />
  <Store v-show="activeSection === 'store'" />
  <Pokedex v-show="activeSection === 'pokedex'" />

</template>
<script>
import Store from './components/Store.vue';
import Navbar from './components/Navbar.vue';
import Pokedex from './components/Pokedex.vue';

export default {
  components: {
    Pokedex,
    Store,
    Navbar
  },
  data() {
    return {
      activeSection: ""
    }
  },
  methods: {
    changeSection(sectionName) {
      this.activeSection = sectionName;
    }
  },
}
</script>
<style>

</style>