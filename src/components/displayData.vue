<template>
    
 <getPokeData/> 
    
</template>

<script>
    import getPokeData from './getPokeData.vue';
    export default {
        name: "displayData",

    Components: {   
    },

    data(){
        return{
            PokeNumber: 1,
            pokemonLoaded: false,
            pokemon: {}
        }
    },
    methods: {
        getData(){
            const Pokemon = getPokeData.loadApiNumber();
            this.pokemon = Pokemon;
            this.pokemonLoaded = true;
            return Pokemon;

        }
    },
    }   

</script>