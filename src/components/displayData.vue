<template>

    <div v-if="pokemonLoaded"> {{ this.Pokemon.PokemonName }}  </div>

</template>

<script>
    import getPokeData from './getPokeData.vue';
    export default {
        name: "displayData",

    //Components: {   
   // },

    data(){
        return{
            PokeNumber: 1,
            pokemonLoaded: false,
            pokemon: {}
        }
    },
    methods: {
        /*async setNrPlus() {
            this.PokeNumber = (this.PokeNumber + 1);
            this.loadApiNumber();
        },
        async setNrMinus() {
            this.PokeNumber = (this.PokeNumber - 1);
            this.loadApiNumber();
        },*/
        getData(){
            const Pokemon = getPokeData.loadApiNumber();
            this.pokemon = Pokemon;
            return Pokemon;
        }
    },
    }   

</script>