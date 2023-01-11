<template>

    <button @click="setVisible"> </button>
    
<div v-if="visible"> 
   
{{ this.myPokemonName }}
</div>  

</template>

<script>
    import getPokeData from './getPokeData.vue';
    export default {
        name: "displayData",
        props: {myPokemonName: String}, //unbegrenzt viele möglich
    Components: {   
    },

    data(){
        return{
            PokeNumber: 1,
            visible: true,
            pokemon: {}

        }
    },
    methods: {
        getData(){
            const Pokemon = getPokeData.loadApiNumber();
            this.pokemon = Pokemon;
            this.pokemonLoaded = true;
            return Pokemon;

        },
        setVisible(){
            this.visible=true;
        }
    },
    }   

</script>