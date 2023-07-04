<script>
const endpoint = 'https://41tyokboji.execute-api.eu-central-1.amazonaws.com/dev/api/v1/pokemons';

import AppFilter from './AppFilter.vue';
// Import lybrary Axios
import axios from 'axios';
import { store } from '../assets/data/store.js'


// LOGIC
export default {

    components: { AppFilter },
    created() {

        // Riempio l'array dei pokemons al montare della pagina
        this.getPokemons(endpoint);
        this.isLoading = true;
    },
    data() {
        return {
            pokemons: store.pokemons,
            isLoading: false,
        }
    },
    methods: {

        // Take pokemons from API
        getPokemons(endpoint) {
            axios.get(endpoint)
                .then(res => {
                    this.pokemons = [];
                    res.data.docs.forEach(doc => {
                        this.pokemons.push(doc);
                    })
                })
                .catch(err => {
                    console.error(err.message)
                })
                .then(() => {
                    this.isLoading = false
                });
        },

        // Filter by first Type of Pokemons
        filterType1(userSelect) {

            if (!userSelect) return this.getPokemons(endpoint)
            const filteredEndpoint = endpoint + `/?eq[type1]=${userSelect}`

            return this.getPokemons(filteredEndpoint)
        },

        // Filter by second Type of Pokemons
        filterType2(userSelect) {

            if (!userSelect) return this.getPokemons(endpoint)
            const filteredEndpoint = endpoint + `/?eq[type2]=${userSelect}`

            return this.getPokemons(filteredEndpoint)
        },

        // Filter by name of Pokemons
        filterText(userText) {
            const filteredEndpoint = endpoint + `/?start[name]=${userText}`
            return this.getPokemons(filteredEndpoint)
        },
    },
    computed: {
        orderedPokemons() {
            return this.pokemons.sort((prev, next) => prev.number > next.number ? 1 : -1)
        },
    }
}
</script>

<template>
    <!-- App For Filter the pokemons card -->
    <AppFilter @change-type1="filterType1" @change-type2="filterType2" @search-text="filterText" @reset-text="filterText" />


    <!-- LOADING -->
    <h1 v-if="isLoading" class="loading">LOADING...</h1>

    <!-- POKEDEX -->
    <div v-else class="container py-5">
        <div class="row row-cols-4 g-4">
            <div v-for="pokemon in orderedPokemons" :key="pokemon._id" class="col">
                <div class="card" :class="pokemon.color">
                    <div class="card-img-top">
                        <img class="img-fluid" :src="pokemon.imageUrl" alt="">
                    </div>
                    <div class="card-body">
                        <p>Number: {{ pokemon.number }}</p>
                        <p>Name: <strong>{{ pokemon.name }}</strong></p>
                        <p>Type: {{ pokemon.type1 }} <span v-if="pokemon.type2">{{ pokemon.type2 }}</span></p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped lang="scss">
.loading {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.5);
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
}

.card {

    &.Green {
        background-color: lightgreen;
    }

    &.Red {
        background-color: lightcoral;
        color: white;

    }

    &.Blue {
        background-color: lightblue;
    }

    &.Black {
        background-color: black;
        color: white;
    }

    &.Yellow {
        background-color: lightyellow;
    }

    &.White {
        background-color: white;
    }

    &.Brown {
        background-color: brown;
        color: white;
    }

    &.Grey {
        background-color: gray;
    }

    &.Pink {
        background-color: pink;
    }

    &.Purple {
        background-color: plum;
        color: white;
    }
}

.card-img-top {
    height: 300px;
    border-bottom: 1px solid lightgray;
    padding: 0 1rem;
    display: flex;
    align-items: center;
    justify-content: center;
}

img {
    max-height: 100%;
}

p {
    align-self: flex-end;
}
</style>