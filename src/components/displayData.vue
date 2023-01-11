<template>
    <div> Pokemon Nummer eingeben: 
        <input id="eingabeId" type="number" v-model="PokeNumber"> </div>
    
      <div class="buttons">
        <button @click="getData"> Eingabe </button>
      </div>
    
    <SearchBar/>
    <div v-if="pokemonLoaded"> {{ PokemonName }}  </div>

</template>

<script>
    import getPokeData from './getPokeData.vue';
    import searchBar from './searchBar.vue';

    export default {
        name: "displayData",

    Components: {
        searchBar
    },
    data(){
        return{
            PokeNumber: 1,
            pokemonLoaded: false,
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
            const Pokemon = getPokeData.loadApiNumber(5);
            return Pokemon;
        }
    },
    }   

</script>