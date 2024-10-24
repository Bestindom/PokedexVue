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
        }
    }
}
</script>
<style></style>
