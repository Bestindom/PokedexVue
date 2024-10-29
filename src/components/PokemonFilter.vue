<template>
    <div class="container">
        <div>
            <h1 class="text-center pt-5 mt-5">Pokédex</h1>
        </div>

        <div class="container my-5">
            <button v-for="type in types" class="btn m-1" :style="{ backgroundColor: type.color }"
                @click="filterPokemons(type.name)">
                <strong>{{ type.name }}</strong>
            </button>
        </div>

    </div>
</template>
<script>
export default {
    data() {
        return {
            filteredPokemons: [],
            favorites: [],
            team: [],
            types: [
                {
                    "name": "normal",
                    "color": "#A8A878"
                },
                {
                    "name": "fire",
                    "color": "#F08030"
                },
                {
                    "name": "water",
                    "color": "#6890F0"
                },
                {
                    "name": "electric",
                    "color": "#F8D030"
                },
                {
                    "name": "grass",
                    "color": "#78C850"
                },
                {
                    "name": "ice",
                    "color": "#98D8D8"
                },
                {
                    "name": "fighting",
                    "color": "#C03028"
                },
                {
                    "name": "poison",
                    "color": "#A040A0"
                },
                {
                    "name": "ground",
                    "color": "#E0C068"
                },
                {
                    "name": "flying",
                    "color": "#A890F0"
                },
                {
                    "name": "psychic",
                    "color": "#F85888"
                },
                {
                    "name": "bug",
                    "color": "#A8B820"
                },
                {
                    "name": "rock",
                    "color": "#B8A038"
                },
                {
                    "name": "ghost",
                    "color": "#705898"
                },
                {
                    "name": "dragon",
                    "color": "#7038F8"
                },
                {
                    "name": "dark",
                    "color": "#705848"
                },
                {
                    "name": "steel",
                    "color": "#B8B8D0"
                },
                {
                    "name": "fairy",
                    "color": "#EE99AC"
                },
                {
                    "name": "Show All",
                    "color": "#EE99AC"
                },
                {
                    "name": "Favorites",
                    "color": "#EE99AC"
                },
                {
                    "name": "Team",
                    "color": "#EE99AC"
                }
            ]
        }
    },
    methods: {
        filterPokemons(typeName) {
            this.$emit("filter", typeName);
        },
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
    }
}
</script>
<style></style>
