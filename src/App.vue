<template>
  <Navbar @section="changeSection" />
  <Store v-show="activeSection === 'store'" />
  <!-- change to v-if -->
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
      activeSection: "pokedex"
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