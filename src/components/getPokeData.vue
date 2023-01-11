<template>

    <div> Pokemon Nummer eingeben: 
        <input type="number" v-model="PokeNumber"> 
        <button @click="loadApiNumber">Click..</button>
    </div>

    <div>  
        <button @click="setNrPlus">hoch </button>
        <button @click="setNrPlus">runter</button>
    </div>


</template>

<script>
import axios from 'axios'

export default {
    name: 'getPokeData',
    components: {

    },
    data(){ 
    return {
      PokeNumber: 1,
    }
    },
    methods: {
        async loadApiNumber(){
            await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.PokeNumber}`).then((response) => {
                console.log(response);
                const returnData = response.data;
                return returnData;
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
  