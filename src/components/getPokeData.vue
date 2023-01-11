<template>

    <div> Pokemon Nummer eingeben: 
        <input type="number" v-model="PokeNumber"> 
        <button @click="loadApiNumber"> Button </button>
    </div>

    <div>  
        <button @click="setNrPlus">hoch </button>
        <button @click="setNrMinus">runter</button>
    </div>

    <displayData v-if="pokemonloaded"/>

</template>

<script>
import axios from 'axios'


export default {
    name: 'getPokeData',
    props: {pokeData: Object},
    // components: {
    //},
    data(){ 
    return {
      returnData: null,
      PokeNumber: 1,
      pokemonLoaded: false,
      height: 0,
    }
    },
    methods: {
        async loadApiNumber(){
            await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.PokeNumber}`).then((response) => {
                console.log(response);
                this.height = response.data;
                return;
    })
    },
    async setNrPlus() {
        this.PokeNumber = (this.PokeNumber + 1);
        this.loadApiNumber();
    },
    async setNrMinus() {
        this.PokeNumber = (this.PokeNumber - 1);
        this.loadApiNumber();
    },
}
}   
</script>

<style>
    .getPokeData{
        list-style: none;
    }
</style>
  