<template>
  <div>
    <h1 class="text-center m-5">Store</h1>
  </div>
  <Store>

  </Store>
  <div>
    <h1 class="text-center m-5">Pokédex</h1>
  </div>
  <PokemonFilter @filter="filterByType">

  </PokemonFilter>
  <div class="container">
    <div class="row">
      <div v-for="pokemon in filteredPokemons" class="col-md-4">
        <div class="card mb-4">
          <img :src="pokemon.sprites.front_default" class="card-img-top" alt="pokemon">

          <!-- favorite button -->
          <button class="btn position-absolute top-0 end-0" @click="addToFavorites(pokemon)">
            <i v-if="isFavorite(pokemon)" class="bi bi-star-fill yellow-star"></i>
            <i v-else class="bi bi-star"></i>
          </button>

          <!-- team button -->
          <button class="btn position-absolute top-0 end-90" @click="addToTeam(pokemon)">
            <i v-if="inTeam(pokemon)" class="bi bi-person-plus-fill"></i>
            <i v-else class="bi bi-person-plus"></i>
          </button>

          <div class="card-body">
            <h5 class="card-title">{{ pokemon.name }}</h5>
            <button v-for="type in pokemon.types" :key="type.slot" class="btn me-1"
              :style="{ backgroundColor: typeColors[type.type.name] }">
              {{ type.type.name }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>
<script>
import axios from 'axios';
import PokemonFilter from './components/PokemonFilter.vue';
import Store from './components/Store.vue';

export default {
  components: {
    PokemonFilter,
    Store
  },
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      items: [],
      favorites: [],
      team: [],
      myModal: {},
      typeColors: {
        "normal": "#A8A878",
        "fire": "#F08030",
        "water": "#6890F0",
        "electric": "#F8D030",
        "grass": "#78C850",
        "ice": "#98D8D8",
        "fighting": "#C03028",
        "poison": "#A040A0",
        "ground": "#E0C068",
        "flying": "#A890F0",
        "psychic": "#F85888",
        "bug": "#A8B820",
        "rock": "#B8A038",
        "ghost": "#705898",
        "dragon": "#7038F8",
        "dark": "#705848",
        "steel": "#B8B8D0",
        "fairy": "#EE99AC"
      }
    }
  },
  methods: {

    addToFavorites(pokemon) {
      // findIndex devuelve -1 si no encuentra al pokemon
      const index = this.favorites.findIndex(favorite => favorite.name === pokemon.name);
      if (index === -1) {
        this.favorites.push(pokemon);
        console.log('¡Se ha agregado a favoritos:', pokemon.name, '!');
      } else {
        this.favorites.splice(index, 1);
        console.log('¡Se ha quitado:', pokemon.name, '!');
      }
      console.log(this.favorites);
    },

    isFavorite(pokemon) {
      // Iteración de la lista favorites (es como un forEach) -> return true/false
      return this.favorites.some(favorite => favorite.name === pokemon.name);
    },

    addToTeam(pokemon) {

      const index = this.team.findIndex(team => team.name === pokemon.name);
      if (index === -1) {
        if (this.team.length == 6) {
          alert('Your team is full')
        } else {
          this.team.push(pokemon);
          console.log('¡Se ha agregado a tu team:', pokemon.name);
        }
      } else {
        this.team.splice(index, 1);
        console.log('¡Se ha eliminado del team:', pokemon.name);
      }

      console.log(this.team);
    },

    inTeam(pokemon) {
      return this.team.some(team => team.name === pokemon.name);
    },

    filterByType(typeName) {
      if (typeName === "" || typeName === "Show All") {
        this.filteredPokemons = this.pokemons;
      } else if (typeName == "Favorites") {
        this.filteredPokemons = this.favorites;
      } else if (typeName == "Team") {
        this.filteredPokemons = this.team;
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon =>
          pokemon.types.some(type => type.type.name === typeName)
        );
      }
    },

    getPokemons() {
      const me = this
      axios
        .get('https://pokeapi.co/api/v2/pokemon?limit=151')
        .then(response => {
          const pokemons = response.data.results;

          pokemons.forEach(pokemon => {
            axios
              .get(pokemon.url)
              .then(response => {
                const pokeData = response.data;
                me.pokemons.push(pokeData);
                // console.log(me.pokemons)
                // console.log(pokeData);
              })
              .catch(error => {
                console.error('Error al obtener los datos del Pokémon:', error);
              });
          });
        })
        .catch(error => {
          console.error('Error al obtener la lista de Pokémon:', error);
        });
      this.filteredPokemons = this.pokemons;
    },
  },
  created() {
    this.getPokemons();
  },
}
</script>
<style scoped>
.yellow-star {
  color: #FFDB58;
}
</style>