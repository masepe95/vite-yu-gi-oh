<script>

// Import lybrary Axios
import axios from 'axios';

// LOGIC
export default {

    created() {

        // Riempio l'array dei pokemons al montare della pagina
        this.getPokemons();
    },
    data() {
        return {
        }
    },
    props: {
        pokemons: Array
    },
    methods: {
        getPokemons() {
            axios.get('https://41tyokboji.execute-api.eu-central-1.amazonaws.com/dev/api/v1/pokemons').then(
                (res) => {
                    res.data.docs.forEach(doc => {
                        this.pokemons.push(doc);

                    });
                }
            )
        },
    },
}
</script>

<template>
    <div class="container py-5">
        <h1 class="text-center mb-5">POKEDEX</h1>
        <div class="row row-cols-5 g-4">
            <div v-for="pokemon in pokemons" :key="pokemon.number" class="col">
                <div class="card">
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

<style>
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