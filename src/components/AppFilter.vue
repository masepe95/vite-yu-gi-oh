<script>
const endpoint = 'https://41tyokboji.execute-api.eu-central-1.amazonaws.com/dev/api/v1/pokemons';

import axios from 'axios';
import { store } from '../assets/data/store.js'

export default {
    created() {
        this.getPokemonsType('types1')
        this.getPokemonsType('types2')
    },
    data: () => {
        return {
            options1: store.options,
            options2: store.options2,
            userSelect1: '',
            userSelect2: '',
            userText: '',
        }
    },
    methods: {
        getPokemonsType(type) {
            if (type === 'types1') {
                axios.get(`${endpoint}/${type}`).then(res => {
                    res.data.forEach(option => {
                        this.options1.push(option);
                    })
                })
            } else if (type === 'types2') {
                axios.get(`${endpoint}/${type}`).then(res => {
                    res.data.forEach(option => {
                        this.options2.push(option);
                    })
                })
            }
        },
    },
    emits: ['change-type1', 'change-type2', 'search-text', 'reset-text']
}
</script>

<template>
    <div class="d-flex">

        <!-- FIRST SELECT FOR FIRST TYPE -->
        <select class="form-select w-25" v-model="userSelect1" @change="$emit('change-type1', userSelect1)">
            <option value="">First Type</option>
            <option v-for="option in options1">{{ option }}</option>
        </select>

        <!-- SECOND SELECT FOR SECOND TYPE -->
        <select class="form-select w-25" v-model="userSelect2" @change="$emit('change-type2', userSelect2)">
            <option value="">Second Type</option>
            <option v-for="option in options2">{{ option }}</option>
        </select>

        <!-- INPUT FOR FILTER BY TEXT -->
        <input v-model="userText" @keyup="$emit('search-text', userText)" type="text" placeholder="Cerca..."
            class="form-control ms-5 w-25">
        <span class="ms-2" title="Delete filter">
            <font-awesome-icon @click="$emit('reset-text', userText = '')" icon="fa-solid fa-xmark" size="2xl"
                style="color:red; cursor:pointer;" />
        </span>
    </div>
</template>

<style scoped>
select {
    margin-left: 30px;
}
</style>